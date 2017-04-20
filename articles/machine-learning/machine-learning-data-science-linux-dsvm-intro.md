<properties
    pageTitle="Inrichten van de gegevens van Linux wetenschappelijke virtuele Machine | Microsoft Azure"
    description="Configureer en een Linux gegevens wetenschappelijke virtuele Machine op Azure moet u doen analyses en machine learning maken."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="bradsev" />

# <a name="provision-the-linux-data-science-virtual-machine"></a>De gegevens van Linux wetenschappelijke virtuele Machine inrichten

De Linux gegevens wetenschappelijke virtuele Machine is een Azure virtuele machine die wordt geleverd met een verzameling vooraf geïnstalleerde hulpprogramma's. Deze hulpprogramma's zijn vaak gebruikt voor het uitvoeren van gegevens analyses en machine learning. De belangrijkste softwareonderdelen opgenomen zijn:

- Microsoft R Server Developer Edition
- Anaconda Python verdeling (versie 2.7 en 3.5), met inbegrip van populaire gegevensanalyse bibliotheken
- JupyterHub - een database voor meerdere gebruikers Jupyter notitieblok server ondersteunende R, Python, Julia kernels
- Azure opslag Explorer
- Azure opdrachtregel-interface (CLI) voor het beheren van Azure resources
- PostgresSQL-Database
- Learning gereedschapsmachines
    - [Rekenkundige netwerk Toolkit (CNTK)](https://github.com/Microsoft/CNTK): een diep zal het leren van software toolkit van Microsoft Research.
    - [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): een snelle machine learning-technieken zoals online, hashing, allreduce, kortingen, learning2search, actief, ondersteunende systeem en interactieve onderwijs.
    - [XGBoost](https://xgboost.readthedocs.org/en/latest/): een hulpmiddel snel en nauwkeurig gestimuleerd structuur-implementatie.
    - [Rattle](http://rattle.togaware.com/) (de R Analytical hulpmiddel naar informatie over eenvoudig): een hulpmiddel waarmee aan de slag met gegevens analyses en machine learning in R eenvoudig, met gebruikersinterface gebaseerde gegevens verkennen en modellering met automatisch R-code genereren.
- Azure SDK in Java, Python, node.js, Ruby, PHP
- Bibliotheken in R en Python voor gebruik in Azure Machine Learning en andere Azure services
- Hulpmiddelen voor de ontwikkeling en editors (Eclips, Emacs, gedit, vi)

Gegevens wetenschappelijke doen heeft betrekking op herhalen op een reeks taken:

1. Zoeken, laden en voorverwerking gegevens
2. Maken en testen van modellen
3. De modellen voor verbruik in intelligente toepassingen implementeren

Verschillende hulpmiddelen voor gebruik gegevens wetenschappers deze taken uit te voeren. U kunt veel tijd in beslag nemen om te zoeken naar de juiste versies van de software en vervolgens als wilt downloaden, compileren en deze versies installeren.

De Linux gegevens wetenschappelijke virtuele Machine kunt dit last aanzienlijk vereenvoudigen. Deze gebruiken om snel uw analytics-project. U kunt werken aan taken in verschillende talen, waaronder R, Python SQL, Java en C++. Eclips biedt een IDE voor het ontwikkelen en testen van uw code die is eenvoudig te gebruiken. De SDK Azure is opgenomen in de VM kunt u uw toepassingen maken met behulp van de verschillende services op Linux voor het Microsoft cloud-platform. U hebt ook toegang tot andere talen zoals Ruby, Perl, PHP en node.js die zijn ook vooraf geïnstalleerd.

Er zijn geen kosten software voor deze gegevens wetenschappelijke VM afbeelding. U betalen alleen de Azure hardware-gebruikskosten die worden beoordeeld op basis van de grootte van de virtuele machine die u met de afbeelding VM inrichten. Klik op de [vermelding van de pagina op de Azure Marketplace VM ](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/)vindt u meer informatie over het berekenen in rekening gebracht.


## <a name="prerequisites"></a>Vereisten voor

Voordat u een Linux gegevens wetenschappelijke virtuele Machine maken kunt, moet u hebt het volgende:

- **Een Azure-abonnement**: Zie voor een [gratis proefversie van Azure ophalen](https://azure.microsoft.com/free/).
- **Een Azure opslag-account**: als u wilt maken, raadpleegt u [een Azure opslag-account maken](storage-create-storage-account.md#create-a-storage-account). U kunt ook de opslagruimte account kan worden gemaakt als onderdeel van het proces van het maken van de VM, als u niet wilt gebruiken van een bestaand account.


## <a name="create-your-linux-data-science-virtual-machine"></a>Uw gegevens Linux wetenschappelijke virtuele Machine maken

Hier volgen de stappen voor het maken van een exemplaar van de Linux wetenschappelijke Virtual Machine gegevens:

1.  Navigeer naar de vermelding van de [portal van Azure](https://portal.azure.com/#create/microsoft-ads.linux-data-science-vmlinuxdsvm)virtuele-machine.
2.   Klik op **maken** (onderaan) de wizard weer te geven. ![configureren-gegevens-wetenschappelijke-vm](./media/machine-learning-data-science-linux-dsvm-intro/configure-linux-data-science-virtual-machine.png)
3.   De volgende secties bevatten de ingangen voor elk van de stappen in de wizard (opgesomde aan de rechterkant van de bovenstaande afbeelding) gebruikt om te maken van Microsoft gegevens wetenschappelijke VM. Hier volgen de invoer die nodig zijn voor het configureren van elk van deze stappen:

    een. **Basisbeginselen**:

  - **Naam**: naam van uw gegevens wetenschappelijke-server die u maakt.
  - **Gebruikersnaam in te voeren**: eerste account aanmelden ID.
  - **Wachtwoord**: eerste accountwachtwoord (u kunt SSH openbare sleutel gebruiken in plaats van wachtwoord).
  - **Abonnement**: als u meer dan één abonnement hebt, selecteert u de een waarop de computer is kan worden gemaakt en gefactureerd. Bevoegdheden van de resource maken voor dit abonnement, moet u hebben.
  - **Resourcegroep**: U kunt een nieuw account maken of een bestaande groep gebruiken.
  - **Locatie**: Selecteer het datacenter dat is het meest geschikt. Gewoonlijk is het datacenter dat het grootste deel van uw gegevens zijn, of zich het dichtst bij de fysieke locatie voor snelste netwerktoegang.

    b. **Grootte**:

  - Selecteer een van de servertypen die voldoet aan uw functionele eisen en kostenbeperkingen. Selecteer **Alles** om meer opties van VM grootten te bekijken.

    c. **Instellingen**:

  - **Type schijf**: kies **Premium** als u liever een effen state schijf (SSD). Kies anders **standaard**.
  - **Opslag-Account**: U kunt een nieuw Azure opslag-account maken van uw abonnement of een bestaande eigenschap op dezelfde locatie die is gekozen gebruiken in de stap koppeling de **Basisbeginselen** van de wizard.
  - **Andere parameters**: In de meeste gevallen u alleen de standaardwaarden gebruiken. Niet-standaardwaarden overwegingen, plaats de muisaanwijzer boven de informatieve koppeling voor hulp bij de specifieke velden.

    d. **Overzicht**:

  - Controleer of alle gegevens die u hebt ingevoerd klopt.

    e. **Kopen**:

  - Als u wilt de inrichting, klikt u op **kopen**. Een koppeling is opgegeven met de voorwaarden van de transactie. De VM heeft geen eventuele extra kosten voorbij de berekeningscluster voor de servergrootte die u hebt gekozen in de stap **grootte** .

De inrichting van moet ongeveer 10-20 minuten duren. De status van de inrichting wordt weergegeven op de Azure-portal.

## <a name="how-to-access-the-linux-data-science-virtual-machine"></a>Toegang tot de Linux gegevens wetenschappelijke virtuele Machine

Nadat de VM is gemaakt, kunt u zich aanmeldt bij deze met behulp van SSH. De referenties die u hebt gemaakt in de sectie **Basisbeginselen** van stap 3 voor de tekst shell-interface gebruikt. In Windows, kunt u een hulpmiddel SSH-client zoals [stopverf](http://www.putty.org)downloaden. Als u liever een grafische bureaublad (Windows-systeem, X), kunt u X11 doorschakelen op stopverf of de X2Go-client installeren.

>[AZURE.NOTE] De client X2Go uitgevoerd aanzienlijk beter dan X11 doorsturen in testen. Het is raadzaam om met behulp van de client X2Go voor een grafische gebruikersinterface voor het bureaublad.


## <a name="installing-and-configuring-x2go-client"></a>Installeren en configureren van X2Go-client

De Linux VM, is al ingerichte met X2Go-server en klaar voor het accepteren van clientverbindingen. Als u wilt verbinden met het Linux VM grafische bureaublad, moet u het volgende op de klant doen:

1. Download en installeer de X2Go-client voor uw platform client van [X2Go](http://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. De client X2Go uitvoeren en selecteer **Nieuwe sessie**. Wordt een configuratievenster met meerdere tabbladen geopend. Voer de volgende configuratieparameters in:
    * **Tabblad sessie**:
        - **Host**: de hostnaam of IP-adres van uw Linux gegevens wetenschappelijke VM.
        - **Login**: gebruikersnaam in te voeren op de VM Linux.
        - **SSH poort**: laten staan 22, de standaardwaarde.
        - **Type sessie**: Wijzig de waarde in XFCE. De VM Linux ondersteunt momenteel alleen XFCE bureaublad.
    * **Tabblad Media**: U kunt uitschakelen geluid ondersteuning en client afdrukken als u niet nodig hebt u ze kunt gebruiken.
    * **Gedeelde mappen**: als u mappen van uw clientcomputers dat is gekoppeld aan de VM Linux, plaatst u de mappen op de client computer die u wilt delen met de VM op dit tabblad.

Nadat u zich aanmeldt bij de VM met behulp van de client SSH of XFCE grafische bureaublad via de client X2Go, bent u klaar om te beginnen met de hulpmiddelen die zijn geïnstalleerd en geconfigureerd op de VM. Klik op XFCE, kunt u toepassingen menu Snelkoppelingen en pictogrammen op het bureaublad bekijken voor veel van de hulpmiddelen.


## <a name="tools-installed-on-the-linux-data-science-virtual-machine"></a>Hulpprogramma's zijn geïnstalleerd op de Linux wetenschappelijke virtuele Machine gegevens

### <a name="microsoft-r-open"></a>Microsoft R openen
R is een van de populairste talen voor gegevensanalyse en machine learning. Als u R gebruiken voor uw analyse wilt, heeft de VM Microsoft R openen (BHT) met de wiskundige Kernel bibliotheek (MKL). De MKL optimaliseert wiskundige bewerkingen in analytical algoritmen gelijke waarden. BHT is 100 procent is compatibel met CRAN-R en een van de R-bibliotheken die zijn gepubliceerd in CRAN kan worden geïnstalleerd op de BHT. U kunt uw R-programma's in een van de standaard-editor, zoals vi, Emacs of gedit bewerken. U kunt ook downloaden en gebruiken van andere IDEs, zoals [RStudio](http://www.rstudio.com). U het uitkomt krijgt u een eenvoudig script (installRStudio.sh) in de adreslijst **/dsvm/tools** die wordt geïnstalleerd RStudio samen. Als u de editor Emacs gebruikt, is werken met R-bestanden in de editor Emacs notitie dat de Emacs ESS (Emacs Speaks Statistiek), waardoor eenvoudiger inpakken vooraf geïnstalleerd geweest.

Als u wilt starten R, typt u **R** alleen in de shell. Hiermee gaat u naar een interactieve omgeving. Voor het ontwikkelen van uw programma R, u meestal gebruikt een editor zoals Emacs of vi of gedit, en voer vervolgens de scripts in R. Als u RStudio hebt geïnstalleerd, hebt u een volledige grafische IDE-omgeving voor het ontwikkelen van uw programma R.

Er is ook een script R voor u bij het installeren van de [belangrijkste 20 R-pakketten](http://www.kdnuggets.com/2015/06/top-20-r-packages.html) als u wilt. Dit script kan worden uitgevoerd nadat u zich in de interface voor interactieve op R, die kan worden opgenomen (zoals genoemd) door te typen **R** in de shell.  

### <a name="python"></a>Python
Voor de ontwikkeling met Python, is Anaconda Python verdeling 2.7 en 3.5 geïnstalleerd. Deze verdeling bevat de basis Python samen met ongeveer 300 van de populairste wiskundige, engineering en gegevens analytics-pakketten. U kunt de standaard teksteditors gebruiken. Bovendien kunt u Spyder, een Python IDE bij Anaconda Python onderzoeken. Spyder moet een grafische desktopcomputer of X11 doorsturen. Een snelkoppeling naar Spyder is opgegeven in het grafische bureaublad.

Aangezien we Python 2.7 en 3.5 hebt, moet u de gewenste Python-versie die u wilt werken in de huidige sessie specifiek te activeren. Het activeringsproces voor wordt de variabele pad naar de gewenste versie van Python.

Als u wilt activeren Python 2.7, voert u het volgende uit de shell:

    source /anaconda/bin/activate root

Python 2.7 is geïnstalleerd op */anaconda/bin*.

Als u wilt activeren Python 3.5, voert u het volgende uit de shell:

    source /anaconda/bin/activate py35


Python 3.5 is geïnstalleerd op */anaconda/envs/py35/bin*.

Als u wilt een interactieve sessie Python roepen, typt u **python** alleen in de shell. Als u zich op een grafische gebruikersinterface of X11 instellen van doorsturen, kunt u **spyder** om te starten de IDE Python typen.

### <a name="jupyter-notebook"></a>Jupyter notitieblok

De verdeling Anaconda is ook wordt geleverd met een notitieblok Jupyter, een omgeving om code en analyse te delen. Het notitieblok Jupyter wordt geopend met JupyterHub. Meld u aan met uw lokale Linux-gebruikersnaam en wachtwoord.

De Jupyter notitieblok server is vooraf geconfigureerd met Python 2, 3 Python en R kernels. Er is een bureaubladpictogram met de naam 'Jupyter Notebook' aan de browser om toegang tot de server notitieblok starten. Als u van de VM via client SSH of X2Go gebruikmaakt, kunt u ook bezoeken [https://localhost:8000 /](https://localhost:8000/) voor toegang tot de Jupyter notitieblok-server.

>[AZURE.NOTE] Ga verder als u eventuele waarschuwingen certificaat verkrijgen.

U kunt de server Jupyter-notitieblok openen vanaf een willekeurige host. Typ *https://\<VM DNS naam of het IP-adres\>: 8000 /*

>[AZURE.NOTE] Poort 8000 wordt geopend in de firewall al dan niet standaard wanneer de VM is ingericht.

We hebben verpakt steekproef notitieblokken--één in Python en een in R. U kunt de koppeling naar de voorbeelden op de startpagina van het notitieblok zien nadat u naar het notitieblok Jupyter verifiëren met behulp van uw lokale Linux-gebruikersnaam en wachtwoord. U kunt een nieuw notitieblok maken door **Nieuw**en klik vervolgens op de juiste taal kernel te selecteren. Als u de knop **Nieuw** niet ziet, klikt u op het pictogram **Jupyter** op boven links als u wilt gaan naar de startpagina van de server notitieblok.


### <a name="ides-and-editors"></a>IDEs en editors

U hebt een keuze van verschillende code editors. Dit geldt ook voor vi/VIM, Emacs, gEdit en Eclips. gEdit Eclips grafische editors zijn en moet u zijn aangemeld bij een grafische bureaublad u ze kunt gebruiken. Deze editors hebt bureaublad en toepassingen snelkoppelingen in menu Start ze.

Zijn de op tekst gebaseerde editors **VIM** en **Emacs** . Emacs, hebben we het pakket van een invoegtoepassing genoemd Emacs Speaks statistieken (ESS) die werken met R binnen de editor Emacs vergemakkelijkt geïnstalleerd. Meer informatie kan worden gevonden op [ESS](http://ess.r-project.org/).

**Eclips** is een bron openen, extensible IDE die ondersteuning biedt voor meerdere talen. De Java ontwikkelaars edition is het exemplaar dat is geïnstalleerd op de VM. Er zijn Plug-ins beschikbaar voor verschillende populaire talen die kunnen worden geïnstalleerd om uit te breiden de Eclips-omgeving. We hebben ook een invoegtoepassing geïnstalleerd in Eclips **Azure Toolkit voor Eclips**genoemd. U kunt maken, ontwikkelen, testen en implementeren van Azure toepassingen met de Eclips ontwikkelomgeving die ondersteuning biedt voor talen zoals Java. Er is ook een **Azure SDK for Java** voor toegang tot verschillende Azure services uit vanuit een Java-omgeving. Meer informatie over Azure toolkit voor Eclips kan worden gevonden op [Azure Toolkit voor Eclips](../azure-toolkit-for-eclipse.md).

**LaTex** wordt geïnstalleerd door het pakket texlive samen met een Emacs invoegtoepassing [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) pakket, waardoor uw documenten LaTex binnen Emacs authoring eenvoudiger.  

### <a name="databases"></a>Databases

#### <a name="postgres"></a>Postgres
De geopende brondatabase **Postgres** is beschikbaar op de VM, met de services worden uitgevoerd en initdb al voltooid. Nog steeds moet u databases en gebruikers maken. Zie de [documentatie van Postgres](https://www.postgresql.org/docs/)voor meer informatie.  

####  <a name="graphical-sql-client"></a>Grafische SQL-client
**Eekhoorn SQL**, een grafische SQL-client is voldaan verbinding maken met verschillende databases (zoals Microsoft SQL Server, Postgres en MySQL) en SQL-query's uitvoeren. U kunt deze uitvoeren vanuit een grafische bureaubladsessie (via de X2Go-client, bijvoorbeeld). Om aan te roepen eekhoorn SQL, kunt u deze starten vanuit het pictogram op het bureaublad of Voer de volgende opdracht op de shell.

    /usr/local/squirrel-sql-3.7/squirrel-sql.sh

Voordat u het eerste gebruik, uw stuurprogramma's of aliassen van de database instellen. De JDBC-stuurprogramma's bevinden zich op:

*/usr/share/Java/jdbcdrivers*

Zie voor meer informatie [Eekhoorn SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Opdrachtregel hulpmiddelen voor toegang tot Microsoft SQL Server

Het pakket ODBC-stuurprogramma voor SQL Server is ook wordt geleverd met twee opdrachtregel's:

**BCP**: de bcp hulpprogramma bulksgewijs kopieert gegevens tussen een exemplaar van Microsoft SQL Server en een gegevensbestand met in een door de gebruiker opgegeven notatie. Het hulpprogramma bcp kan worden gebruikt met grote aantallen nieuwe rijen in SQL Server-tabellen importeren en exporteren van gegevens uit tabellen in gegevensbestanden. Om gegevens te importeren in een tabel, moet u een bestand-indeling gemaakt voor die tabel of informatie over de structuur van de tabel en de soorten gegevens die geldig voor de kolommen zijn.

Zie [verbinding maken met bcp](https://msdn.microsoft.com/library/hh568446.aspx)voor meer informatie.

**sqlcmd**: U kunt Transact-SQL-instructies opgeven met het hulpprogramma sqlcmd, evenals de systeem procedures en script bestanden bij de opdrachtprompt. Dit hulpprogramma wordt ODBC Transact-SQL-batches uitvoeren.

Zie [verbinding maken met sqlcmd](https://msdn.microsoft.com/library/hh568447.aspx)voor meer informatie.

>[AZURE.NOTE] Er zijn enkele verschillen in dit hulpprogramma tussen Linux en Windows-platforms. Zie de documentatie voor meer informatie.


#### <a name="database-access-libraries"></a>Database access bibliotheken

Bibliotheken zijn beschikbaar in R en Python naar access-databases.

- In R, het **RODBC** of **dplyr** pakket Hiermee kunt u query of SQL-instructies uitvoeren op de databaseserver.
- In Python biedt de bibliotheek **pyodbc** toegang tot de database met behulp van ODBC als de onderliggende laag.  

Toegang krijgen tot **Postgres**:

- Gebruik het pakket **RPostgreSQL**in tekst.
- Gebruik de bibliotheek **psycopg2** in Python:.


### <a name="azure-tools"></a>Azure hulpmiddelen
De volgende Azure hulpprogramma's zijn geïnstalleerd op de VM:

- **Azure opdrachtregel**: het Azure CLI, kunt u maken en beheren van Azure bronnen via shell-opdrachten. Als u wilt de Azure hulpmiddelen roepen, typt u **azure help**. Zie de [Azure CLI documentatiepagina](../virtual-machines-command-line-tools.md)voor meer informatie.
- **Microsoft Azure opslag Explorer**: Microsoft Azure opslag Explorer is een grafische hulpprogramma dat wordt gebruikt om te bladeren door de objecten die u hebt opgeslagen in uw account Azure opslag, en voor het uploaden en downloaden van gegevens naar en vanuit Azure BLOB's. U kunt opslagruimte Explorer openen via het pictogram voor de snelkoppeling op het bureaublad. U kunt deze bij de opdrachtprompt shell oproepen door te typen **StorageExplorer**. U moet zijn aangemeld van een client X2Go, of als zich X11 doorschakelen set up.
- **Azure-bibliotheken**: de volgende zijn enkele van de vooraf geïnstalleerde bibliotheken.

 - **Python**: het Azure-gerelateerde bibliotheken in Python die worden geïnstalleerd zijn **azure**, **azureml**, **pydocumentdb**en **pyodbc**. Met de eerste drie bibliotheken, kunt u opslagruimte Azure-services, Azure Machine Learning en Azure DocumentDB (een NoSQL-database op Azure) openen. De vierde bibliotheek, pyodbc (samen met de ODBC-stuurprogramma voor SQL Server), kunt toegang tot SQL Server Azure SQL-Database en Azure SQL Data Warehouse uit Python met behulp van een ODBC-interface. Voer **pip lijst** als u wilt zien van alle bibliotheken die de lijst. Zorg ervoor dat deze opdracht uitvoert in zowel de 2.7 Python en 3,5 omgevingen.
 - **R**: het Azure-gerelateerde bibliotheken in R die worden geïnstalleerd zijn **AzureML** en **RODBC**.
 - **Java**: de lijst met Azure Java bibliotheken vindt u in de directory **/dsvm/sdk/AzureSDKJava** op de VM. De belangrijkste bibliotheken zijn Azure opslag en het beheer API's, DocumentDB en JDBC-stuurprogramma's voor SQL Server.  

U kunt de [Azure-portal](https://portal.azure.com) openen vanuit de vooraf geïnstalleerde Firefox-browser. In de portal Azure, kunt u maken, beheren en controleren Azure bronnen.

### <a name="azure-machine-learning"></a>Azure Machine Learning

Azure Machine Learning is een volledig beheerde cloudservice waarmee u kunt bouwen, en bekijk analyses oplossingen delen. U maken uw experimenten en modellen van Azure Machine Learning Studio. Dit zijn toegankelijk vanaf een webbrowser op de gegevens wetenschappelijke virtuele machine via [Microsoft Azure Machine Learning](https://studio.azureml.net).

Nadat u zich bij Azure Machine Learning Studio aanmelden, hebt u toegang tot een tekenpapier experimenten waarbij u een logische volgorde voor de machine learning-algoritmen kunt maken. U ook toegang hebt tot een Jupyter notitieblok die worden gehost op Azure Machine Learning en naadloos kunt werken met de experimenten in Machine Learning Studio. Mogelijk maken de machine learning-modellen die u hebt gemaakt door deze in een webservice-interface voor tekstterugloop. Hierdoor kunnen clients geschreven in een taal aan te roepen voorspellingen uit de machine learning-modellen. Zie de [documentatie van Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/)voor meer informatie.

U kunt ook uw modellen in R of Python de VM combineren en deze vervolgens implementeert in productie op Azure Machine Learning. We hebben bibliotheken in R (**AzureML**) en Python (**azureml**) zodat deze functionaliteit geïnstalleerd.

Zie voor informatie over het implementeren van gegevensmodellen in R en Python in Azure Machine Learning [tien wat u in de wetenschappelijke gegevens VM doen kunt](machine-learning-data-science-vm-do-ten-things.md) (met name, de sectie ' bouwen modellen met R of Python en ze mogelijk te maken met Azure Machine Learning ").

>[AZURE.NOTE] Deze instructies zijn voor het Windows-versie van de gegevens wetenschappelijke VM geschreven. Maar de verstrekte er over het implementeren van modellen naar Azure Machine Learning is van toepassing op de VM Linux.

### <a name="machine-learning-tools"></a>Learning gereedschapsmachines

De VM wordt geleverd met een paar machine learning-hulpprogramma's en algoritmen die zijn vooraf gecompileerd en lokaal vooraf is geïnstalleerd. Hierbij:

* **CNTK** (Rekenkundige netwerk Toolkit van Microsoft Research): een diep zal het leren van toolkit.
* **Vowpal Wabbit**: een snelle online learning algoritme.
* **xgboost**: een hulpmiddel waarmee geoptimaliseerde, gestimuleerd structuur algoritmen.
* **Python**: Anaconda Python wordt geleverd met machine learning algoritmen met bibliotheken zoals Scikit meer. U kunt andere bibliotheken installeren met behulp van de `pip install` opdracht.
* **R**: een uitgebreide bibliotheek machine learning-functies is beschikbaar voor R. Sommige van de bibliotheken die vooraf geïnstalleerd zijn zijn lm, glm, randomForest, rpart. Andere bibliotheken kunnen worden geïnstalleerd door te voeren:

        install.packages(<lib name>)

Hier vindt u meer aanvullende informatie over de eerste drie machine learning-hulpmiddelen in de lijst.

#### <a name="cntk"></a>CNTK
Dit is een bron openen, uitgebreide zal het leren van toolkit. Er is een hulpprogramma voor de opdrachtregel (cntk), en is al in het pad.

Uitvoeren om uit te voeren op een eenvoudig voorbeeld, de volgende opdrachten in de shell:

    # Copy samples to your home directory and execute cntk
    cp -r /dsvm/tools/CNTK-2016-02-08-Linux-64bit-CPU-Only/Examples/Other/Simple2d cntkdemo
    cd cntkdemo/Data
    cntk configFile=../Config/Simple.cntk

De uitvoer model is in *~/cntkdemo/Output/Models*.

Zie het gedeelte CNTK van [GitHub](https://github.com/Microsoft/CNTK)en de [CNTK wiki](https://github.com/Microsoft/CNTK/wiki)voor meer informatie.


#### <a name="vowpal-wabbit"></a>Vowpal Wabbit

Vowpal Wabbit is een machine learning-systeem met technieken zoals online, hashing, allreduce, kortingen, learning2search, actief, en interactieve onderwijs.

Ga als volgt te werk om het hulpprogramma uitvoert op een eenvoudig voorbeeld:

    cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
    cd vwdemo
    vw house_dataset

Er zijn andere, groter demo's in die map. Zie voor meer informatie over VW, [deze sectie van GitHub](https://github.com/JohnLangford/vowpal_wabbit)en de [Vowpal Wabbit wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki).

#### <a name="xgboost"></a>xgboost
Dit is een bibliotheek waarin is ontworpen en geoptimaliseerd voor algoritmen gestimuleerd (structuur). Het doel van deze bibliotheek is de grenzen van de berekening van machines push tot het de uiterste naar grootschalige boomstructuur verhogen die nodig is scalable, draagbare en nauwkeurig.

Deze wordt weergegeven als een opdrachtregel, evenals een R-bibliotheek.

Als u wilt deze bibliotheek in R gebruiken, kunt u een interactieve R-sessie starten (alleen door te typen **R** in de shell) en de bibliotheek.

Hier volgt een eenvoudig voorbeeld die r vraag kan worden uitgevoerd:

    library(xgboost)

    data(agaricus.train, package='xgboost')
    data(agaricus.test, package='xgboost')
    train <- agaricus.train
    test <- agaricus.test
    bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                    eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
    pred <- predict(bst, test$data)

Als u wilt de opdrachtregel xgboost uitvoert, moet u hier de opdrachten uitvoeren in de shell zijn:

    cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
    cd xgboostdemo
    xgboost mushroom.conf


Een .model-bestand is opgeslagen in de opgegeven map. Informatie over dit voorbeeld demo vindt u [op GitHub](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Zie voor meer informatie over xgboost, de [xgboost documentatiepagina](https://xgboost.readthedocs.org/en/latest/)en de [Github-bibliotheek](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Rammelaar
Rammelaar ( **R** **A**nalytical **T**Inkthulpprogramma **T**o **L**verdienen **E**asily) wordt gebruikt voor gebruikersinterface gebaseerde gegevens verkennen en modellering. Daaraan statistische en visuele overzichten van gegevens, transformaties gegevens die kunnen gemakkelijk worden gebaseerd, genereert gecontroleerde zowel werkende modellen van de gegevens, de prestaties van modellen grafisch, presenteert en scores nieuwe gegevens worden ingesteld. R-code, repliceren de bewerkingen in de gebruikersinterface die kunnen worden uitgevoerd rechtstreeks in R of kan worden gebruikt als uitgangspunt voor verdere analyse wordt ook gegenereerd.

Als u wilt uitvoeren Rammelaar, moet u zich in een grafische aanmeldingsproblemen bureaubladsessie. Typ op de terminal ```R``` om in te voeren de R-omgeving. Klik bij de prompt R Voer de volgende opdrachten:

    library(rattle)
    rattle()

Een grafische gebruikersinterface verschijnt met een reeks tabbladen. Hier volgen de stappen aan de slag in Rammelaar nodig voor een verzameling voorbeeldgegevens weer het bouwen van een model. In sommige van de onderstaande stappen, wordt u gevraagd automatisch installeren en laden sommige vereiste R-pakketten die nog niet op het systeem.

>[AZURE.NOTE] Als u geen toegang tot het pakket installeren in de systeemmap (de standaardinstelling), ziet u mogelijk gevraagd of op uw R-console-venster pakketten installeren naar uw persoonlijke bibliotheek. Beantwoord *y* als u deze aanwijzingen wordt weergegeven.

1. Klik op **uitvoeren**.
2. Een dialoogvenster verschijnt, waarin u wordt gevraagd als u graag gebruik van de gegevensverzameling voorbeeld weer. Klik op **Ja** om te laden in het voorbeeld.
3. Klik op het tabblad **Model** .
4. Klik op **uitvoeren** om te maken van een beslissingsstructuur.
5. Klik op **Tekenen** om weer te geven van de beslissingsstructuur.
6. Klik op het keuzerondje **bos** en klik op **uitvoeren** om te maken van een willekeurig bos.
7. Klik op het tabblad **evalueren** .
8. Klik op het keuzerondje **risico's** en klikt u op **uitvoeren** om twee risico (cumulatief) prestaties waarnemingspunten weer te geven.
9. Klik op het tabblad **Log** om weer te geven van de code genereren R voor de voorgaande bewerkingen.
(Vanwege een fout in de huidige versie van Rammelaar, moet u invoegen een *#* teken vóór *Dit logboek... exporteren* in de tekst van het logboek.)
10. Klik op de knop **exporteren** om op te slaan de R-script-bestand met de naam *weather_script. R* naar de basismap.

U kunt verlaten Rammelaar en R. U kunt nu de gegenereerde R-script wijzigen of deze gebruiken als ze uit te voeren op elk gewenst moment als u wilt herhalen alles die binnen de gebruikersinterface Rattle werden geëffend. Met name voor beginners in R is dit een eenvoudige manier om snel analyses en machine learning in een eenvoudige grafische interface, terwijl de code automatisch genereert in R om te wijzigen en/of meer.


## <a name="next-steps"></a>Volgende stappen
Hier ziet u hoe u uw leren en te verkennen kunt blijven:

* De [gegevens wetenschappelijke op de Linux wetenschappelijke virtuele Machine gegevens](machine-learning-data-science-linux-dsvm-walkthrough.md) -rondleiding ziet u hoe u verschillende algemene taken in het wetenschappelijke gegevens uitvoeren met de Linux gegevens wetenschappelijke VM deze is ingericht hier. 
* Kennismaken met de verschillende wetenschappelijke hulpmiddelen voor gegevens op de wetenschappelijke gegevens VM door het uitproberen van de hulpmiddelen in dit artikel beschreven. U kunt ook *dsvm-meer-info* uitvoeren op de shell in de virtuele machine voor een eenvoudige introductie en aanwijzers naar meer informatie over de hulpmiddelen op de VM is geïnstalleerd.  
* Leer hoe u analytische end-to-end-oplossingen systematisch kunt maken met behulp van het [Team gegevens wetenschap proces](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).
* Ga naar de [Galerie met Cortana Analytics](http://gallery.cortanaanalytics.com) voor machine learning en gegevens analytics voorbeelden die de Cortana Analytics-Suite gebruiken.
