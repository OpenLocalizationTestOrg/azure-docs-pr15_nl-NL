<properties 
    pageTitle="Inrichten van de gegevens naar Microsoft verzonden wetenschappelijke virtuele Machine | Microsoft Azure" 
    description="Configureer en een gegevens wetenschappelijke virtuele Machine op Azure moet u doen analyses en machine learning maken." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="bradsev" />


# <a name="provision-the-microsoft-data-science-virtual-machine"></a>De gegevens naar Microsoft verzonden wetenschappelijke virtuele Machine inrichten

Microsoft gegevens wetenschappelijke VM is een afbeelding Azure virtuele machine (VM) vooraf geïnstalleerd en geconfigureerd met enkele populaire programma's die vaak worden gebruikt voor gegevens analyses en machine learning. De hulpprogramma's zijn:

- Microsoft R Server Developer Edition
- Anaconda Python verdeling
- Jupyter notitieblok (met R, Python kernels)
- Visual Studio-Community-editie
- Power BI desktop
- SQL Server 2016 Developer Edition
- Learning gereedschapsmachines
    - [Rekenkundige netwerk Toolkit (CNTK)](https://github.com/Microsoft/CNTK): een diep zal het leren van software toolkit van Microsoft Research.
    - [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): een snelle machine learning-technieken zoals online, hashing, allreduce, kortingen, learning2search, actief, ondersteunende systeem en interactieve onderwijs.
    - [XGBoost](https://xgboost.readthedocs.org/en/latest/): een hulpmiddel snel en nauwkeurig gestimuleerd structuur-implementatie.
    - [Rattle](http://rattle.togaware.com/) (de R Analytical hulpmiddel naar informatie over eenvoudig): een hulpmiddel waarmee aan de slag met gegevens analyses en machine learning in R eenvoudig, met gebruikersinterface gebaseerde gegevens verkennen en modellering met automatisch R-code genereren.
    - [mxnet](https://github.com/dmlc/mxnet): een uitgebreide learning framework ontworpen voor zowel efficiëntie en flexibiliteit
- Bibliotheken in R en Python voor gebruik in Azure Machine Learning en andere Azure services
- Cijfer inclusief cijfer we vaker doen om te werken met bron code opslagplaatsen inclusief GitHub, Visual Studio Team Services
- Windows-poorten van verschillende populaire Linux hulpprogramma (inclusief awk, sed, perl, grep, zoeken, wget, krul enzovoort) toegankelijk is via de opdrachtprompt. 


Gegevens wetenschappelijke doen heeft betrekking op herhalen op een reeks taken: zoeken, laden en vooraf verwerking gegevens, bouwen en implementeren van de modellen voor verbruik in intelligente toepassingen modellen testen. Gegevens wetenschappers gebruik diverse hulpprogramma's om deze taken te voltooien. Het kan zijn heel veel tijd in beslag naar de juiste versies van de software, zoeken en klik vervolgens download en installeer deze. Microsoft gegevens wetenschappelijke VM kunt dit last vereenvoudigen met behulp van de afbeelding van een kant-en-klare die kan worden ingericht op Azure die alle verschillende populaire gereedschap vooraf geïnstalleerd en geconfigureerd. 

Microsoft gegevens wetenschappelijke VM jump-starts uw analytics-project. U kunt werken aan taken in verschillende talen, met inbegrip van R, Python SQL en C#. Visual Studio biedt een IDE voor het ontwikkelen en testen van uw code die is eenvoudig te gebruiken. De SDK Azure is opgenomen in de VM kunt u uw toepassingen met behulp van de verschillende services op de Microsoft cloud-platform maken. 

Er zijn geen kosten software voor deze gegevens wetenschappelijke VM afbeelding. U betaalt alleen voor de Azure gebruikskosten welke afhankelijk van de grootte van de virtuele machine die u inrichten. Meer informatie over het berekenen in rekening gebracht vindt u in de detailsectie van prijzen op de pagina [gegevens wetenschappelijke virtuele Machine](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) . 


## <a name="prerequisites"></a>Vereisten voor

Voordat u een Microsoft gegevens wetenschappelijke virtuele Machine maken kunt, moet u hebt het volgende:

- **Een Azure-abonnement**: Zie voor een [gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

*   **Een Azure opslag-account**: als u wilt maken, raadpleegt u [een Azure opslag-account maken](storage-create-storage-account.md#create-a-storage-account). Het account opslag kan ook worden gemaakt als onderdeel van het proces van het maken van de VM als u niet wilt gebruiken van een bestaand account.


## <a name="create-your-microsoft-data-science-virtual-machine"></a>Uw gegevens naar Microsoft verzonden wetenschappelijke virtuele Machine maken

Hier volgen de stappen voor het maken van een exemplaar van Microsoft gegevens wetenschappelijke VM:

1.  Navigeer naar de virtuele machine vermelding op [Azure-portal](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).
2.   Selecteer de knop **maken** onderaan in de wizard een moeten worden genomen. ![configureren-gegevens-wetenschappelijke-vm](./media/machine-learning-data-science-provision-vm/configure-data-science-virtual-machine.png)
3.   De wizard gebruikt om te maken van Microsoft gegevens wetenschappelijke VM vereist **invoeritems** voor elk van de **vijf stappen** opgesomde aan de rechterkant van de volgende afbeelding. Hier volgen de invoer die nodig zijn voor het configureren van elk van deze stappen:
    
    1.   **Basisbeginselen**
        1.   **Naam**: naam van uw gegevens wetenschappelijke-server die u maakt.
        2.   **Gebruikersnaam in te voeren**: beheerder account aanmeldings-id.
        3.   **Wachtwoord**: beheerderswachtwoord-account.
        4.   **Abonnement**: als u meer dan één abonnement hebt, selecteert u de een waarop de computer is kan worden gemaakt en gefactureerd.
        5.   **Resourcegroep**: U kunt een nieuw account maken of een bestaande groep gebruiken.
        6.   **Locatie**: Selecteer het datacenter dat is het meest geschikt. Gewoonlijk is het datacenter dat het grootste deel van uw gegevens heeft of zich het dichtst bij de fysieke locatie voor snelste netwerktoegang.
         
    2.   **Grootte**: Selecteer een van de servertypen die voldoet aan uw functionele eisen en kostenbeperkingen. U kunt meer opties van VM grootten krijgen door in te schakelen 'Alles'.
    
    3.   **Instellingen**:
        1.   **Type schijf**: Kies Premium als u liever een solid-state drive (SSD), anders kiest u "Standaard".
        2.   **Opslag-Account**: U kunt een nieuw Azure opslag-account maken van uw abonnement of een bestaande eigenschap op dezelfde *locatie* die is gekozen gebruiken in de stap koppeling de **Basisbeginselen** van de wizard.
        3.   **Andere parameters**: meestal dat u de standaardwaarden alleen gebruiken. U kunt de muisaanwijzer boven de informatieve koppeling voor hulp bij de specifieke velden voor het geval u wilt Houd rekening met het gebruik van niet-standaardwaarden.

    4.   **Overzicht**: Controleer of alle gegevens die u hebt ingevoerd klopt.
    5.   **Kopen**: klik op **kopen** om te beginnen met de inrichting van. Een koppeling is opgegeven met de voorwaarden van de transactie. De VM heeft geen eventuele extra kosten voorbij de berekeningscluster voor de servergrootte die u hebt gekozen in de stap **grootte** . 


>[AZURE.NOTE] De inrichting van moet ongeveer 10-20 minuten duren. De status van de inrichting wordt weergegeven op de Azure-portal.

## <a name="how-to-access-the-microsoft-data-science-virtual-machine"></a>Toegang tot de gegevens voor Microsoft VM wetenschappelijk

Nadat de VM is gemaakt, kunt u extern bureaublad in deze met de beheerdersreferenties voor het account dat u hebt geconfigureerd in de vorige sectie van de **Basisbeginselen** . 

Wanneer uw VM gemaakt en deze is ingericht, bent u gereed om te beginnen met de hulpmiddelen die zijn geïnstalleerd en geconfigureerd op is geïnstalleerd. Er zijn start menu tegels en pictogrammen voor veel van de hulpmiddelen op het bureaublad. 

## <a name="how-to-create-a-strong-password-on-the-jupyter-notebook-server"></a>Het maken van een sterk wachtwoord op de server Jupyter-notitieblok 

Voer de volgende opdracht uit een opdrachtprompt op de gegevens wetenschappelijke virtuele Machine om uw eigen sterk wachtwoord voor de Jupyter notitieblok server is geïnstalleerd op de computer hebt gemaakt.

    c:\anaconda\python.exe -c "import IPython;print IPython.lib.passwd()"

Kies een sterk wachtwoord wanneer u wordt gevraagd.

U ziet de wachtwoordhash in de notatie 'sha1:xxxxxx' in de uitvoer. Kopieer deze wachtwoordhash en vervang van de bestaande hash die zich in uw notitieblok config-bestand zich bevindt: **C:\ProgramData\jupyter\jupyter_notebook_config.py** met een parameter naam ***c.NotebookApp.password***.

Alleen de (xxxxxx) een deel van de bestaande hashwaarde die binnen de aanhalingstekens vervangen. De aanhalingstekens en de ***sha1:*** voorvoegsel voor de waarde van de parameter beide moeten worden bewaard.

Tot slot moet u stoppen en opnieuw starten van de server Jupyter, die wordt uitgevoerd op de VM als een windows geplande taak **Start_IPython_Notebook**genoemd. Als uw nieuwe wachtwoord niet wordt geaccepteerd nadat deze taak opnieuw is opgestart, probeer de lopende python processen van Taakbeheer beëindigen en start de geplande taak. U kunt ook proberen de virtuele machine opgestart.

## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Hulpprogramma's zijn geïnstalleerd op de Microsoft wetenschappelijke virtuele Machine gegevens

### <a name="microsoft-r-server-developer-edition"></a>Microsoft R Server Developer Edition
Als u R gebruiken voor uw analyse wilt, heeft de VM Microsoft R Server Developer edition is geïnstalleerd. Microsoft-R-Server is een ruim zijn bruikbare enterprise-klasse analytics-platform op basis van R die wordt ondersteund, scalable en veilige. Een groot aantal groot gegevens statistieken, bekijk modellering en machine learning-mogelijkheden ondersteunende, ondersteunt R-Server de volledige reeks analytics – verkennen, analyse, visualisatie en modellering. Door en uitbreiden bron openen R, Microsoft R Server volledig compatibel is met R scripts, functies en CRAN-pakketten, om gegevens bij het op schaal enterprise te analyseren. Deze ook adressen de beperkingen in het geheugen van Open bron R door toe te voegen parallelle en gedeelde verwerking van gegevens. Hiermee kunt u veel groter dan wat in het hoofdvenster geheugen past analyses uitvoeren op gegevens.  Visual Studio Community Edition opgenomen in de VM bevat de R hulpprogramma's voor Visual Studio-extensie vindt u een volledige IDE voor het werken met R. U kunt ook downloaden en gebruiken van andere IDEs ook zoals [RStudio](http://www.rstudio.com). 

### <a name="python"></a>Python
Voor de ontwikkeling met Python, is Anaconda Python verdeling 2.7 en 3.5 geïnstalleerd. Deze verdeling bevat de basis Python samen met ongeveer 300 van de populairste wiskundige, engineering en gegevens analytics-pakketten. U kunt Python hulpprogramma's voor Visual Studio (PTVS) die binnen de Visual Studio 2015 Community-editie of op een van de IDEs gebundelde met Anaconda zoals niet-actief of Spyder is geïnstalleerd. U kunt een van de volgende door te zoeken in de zoekbalk starten (**winnen** + **S** -toets).

>[AZURE.NOTE] Om te laten verwijzen de Python hulpprogramma's voor Visual Studio bij Anaconda Python 2.7 en 3.5, moet u aangepaste omgevingen voor elke versie maken. Als u wilt deze paden omgeving instellen in de Visual Studio 2015 Community Edition, Ga naar **Extra** -> **Python hulpmiddelen voor** -> **Python omgevingen** en klik vervolgens op **+ aangepast**. 

Anaconda Python 2.7 onder C:\Anaconda is geïnstalleerd en Anaconda Python 3.5 onder c:\Anaconda\envs\py35 is geïnstalleerd. Zie [PTVS documentatie](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) voor gedetailleerde stappen. 

### <a name="jupyter-notebook"></a>Jupyter notitieblok
Anaconda verdeling is ook wordt geleverd met een notitieblok Jupyter, een omgeving om code en analyse te delen. Een Jupyter notitieblok server is vooraf geconfigureerd met Python 2, 3 Python en R kernels. Er is een bureaubladpictogram met de naam 'Jupyter Notebook aan de browser om toegang tot de server notitieblok starten. Als u van de VM via Extern bureaublad gebruikmaakt, kunt u ook bezoeken [https://localhost:9999 /](https://localhost:9999/) voor toegang tot de Jupyter notitieblok server gebruikt bij aangemeld bij de VM.
 
>[AZURE.NOTE] Ga verder als u eventuele waarschuwingen certificaat verkrijgen. 

We hebben verpakt steekproef notitieblokken in Python en in R. De notitieblokken Jupyter weergeven over het werken met Microsoft R Server, SQL Server 2016 R Services (analytics-database), Python en andere technologieën Azure nadat u zich bij Jupyter aanmelden. U kunt de koppeling naar de voorbeelden op de startpagina van het notitieblok zien nadat u naar het Jupyter-notitieblok met het wachtwoord die u hebt gemaakt in een eerdere stap verifiëren. 

### <a name="visual-studio-2015-community-edition"></a>Visual Studio 2015 Community-editie
Visual Studio-Community-editie geïnstalleerd op de VM. Dit is een gratis versie van de populaire IDE van Microsoft die u voor evaluatie en voor kleine teams gebruiken kunt. U kunt de licentievoorwaarden raadplegen [hier](https://www.visualstudio.com/support/legal/mt171547).  Open Visual Studio door te dubbelklikken op het pictogram bureaublad of het menu **Start** . U kunt ook zoeken naar programma's met **winnen** + **S** en invoeren "Visual Studio". Wanneer er kunt u projecten in talen bijvoorbeeld C#, Python, R, node.js maken kunt. Plug-ins worden ook geïnstalleerd die werkt met Azure services zoals Azure-gegevenscatalogus, Azure HDInsight (Hadoop, een) en Azure gegevens Lake te vergemakkelijken. 

>[AZURE.NOTE] Mogelijk dat u een bericht weergegeven dat de evaluatieperiode is verlopen. Voer de referenties van uw Microsoft-account of maak een nieuwe gratis account toegang krijgen tot de Visual Studio Community Edition. 

### <a name="sql-server-2016-developer-edition"></a>SQL Server-2016 Developer edition
Een developer-versie van SQL Server-2016 met R Services om uit te voeren in de database analytics is beschikbaar op de VM. R-Services bieden een platform voor het ontwikkelen en implementeren van intelligente toepassingen. U kunt de rijke en krachtige R-taal en de veel-pakketten van de community modellen maken en voorspellingen genereren voor uw SQL Server-gegevens. Omdat R-Services (In-database) de taal R met SQL Server integreren, kunt u analytics dicht bij de gegevens behouden. Hierdoor wordt de kosten en beveiligingsrisico's die zijn gekoppeld aan gegevens verplaatsen.

>[AZURE.NOTE] De SQL Server-2016 developer edition kan alleen worden gebruikt voor de ontwikkeling en testdoeleinden. Moet u een licentie om het in productie. 

U kunt de SQL server openen door het starten van **SQL Server Management Studio**. De naam van uw VM wordt gevuld als de naam van de Server. Windows-verificatie gebruiken wanneer u bent aangemeld als beheerder in Windows. Wanneer u SQL Server Management Studio kunt u andere gebruikers te maken, databases maken, gegevens importeren en SQL-query's uitvoeren. 

Om in te schakelen In de database-analyse met Microsoft R, voert u de volgende opdracht uit als een actie in SQL Server management studio na melden als beheerder van de tijd. 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 
        
        (Please replace the %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure 
Verschillende Azure hulpprogramma's zijn geïnstalleerd op de VM:

- Er is een snelkoppeling op het bureaublad voor toegang tot de SDK van Azure-documentatie. 
- **AzCopy**: gebruikt om gegevens en afmelden bij uw Microsoft Azure opslag-Account te verplaatsen. Als u wilt zien gebruik, typt u **Azcopy** opdrachtprompt om te zien van het gebruik. 
- **Microsoft Azure opslag Explorer**: gebruikt om te bladeren door de objecten die u hebt opgeslagen in uw gegevens Azure opslag-Account en doorverbinden naar en vanuit Azure opslag. U kunt **Opslagruimte Explorer** typt in zoeken of vinden in het menu Start van Windows voor toegang tot dit hulpmiddel. 
- **Adlcopy**: gebruikt om gegevens te verplaatsen naar Lake van Azure-gegevens. Als u wilt zien gebruik, typt u **adlcopy** in een opdrachtprompt. 
- **dtui**: gebruikt om gegevens te verplaatsen naar en vanuit Azure DocumentDB, een NoSQL-database op de cloud. Typ **dtui** op opdrachtprompt. 
- **Microsoft Data Management Gateway**: kunt u gegevens verplaatsen tussen on-premises gegevensbronnen en cloud. Deze worden gebruikt in hulpmiddelen voor zoals Factory van Azure-gegevens. 
- **Microsoft Azure Powershell**: een hulpprogramma voor het beheren van uw Azure resources in de Powershell scripttaal ook op uw VM is geïnstalleerd. 

###<a name="power-bi"></a>Power BI

Dashboards en uitstekende visualisaties, zodat is het **Power BI Desktop** geïnstalleerd. Gebruik dit hulpprogramma naar halen gegevens uit verschillende bronnen, aan uw dashboards en rapporten van de auteur en ze te publiceren naar de cloud. Zie de [Power BI](http://powerbi.microsoft.com) -site voor informatie. U kunt Power BI desktop vinden in het menu Start. 

>[AZURE.NOTE] Moet u een Office 365-account voor toegang tot Power BI. 

## <a name="additional-microsoft-development-tools"></a>Aanvullende Microsoft development hulpmiddelen
Het [**Installatieprogramma van Microsoft Web Platform**](https://www.microsoft.com/web/downloads/platform.aspx) kan worden gebruikt om te detecteren en andere Microsoft development hulpmiddelen downloaden. Er is ook een snelkoppeling naar het hulpprogramma dat op het bureaublad Microsoft gegevens wetenschappelijke Virtual Machine.  

## <a name="important-directories-on-the-vm"></a>Belangrijke mappen op de VM

| Item                          | Directory |
| ------------------------------| ---------------- |
|Jupyter notitieblok serverconfiguraties | C:\ProgramData\jupyter |
|Jupyter notitieblok voorbeelden basismap| c:\dsvm\notebooks|
|Andere voorbeelden | c:\dsvm\samples|
| Anaconda (standaard: Python 2.7) | c:\Anaconda |
| Anaconda Python 3.5-omgeving | c:\Anaconda\envs\py35|
|R Server zelfstandige exemplaar directory (standaard R exemplaar) | C:\Program Files\Microsoft SQL Server\130\R_SERVER |
| R-Server In de database-exemplaar adreslijst | C:\Program Files\Microsoft SQL Server\MSSQL13. MSSQLSERVER\R_SERVICES |
| Diverse hulpprogramma 's | c:\dsvm\tools|

>[AZURE.NOTE] Exemplaren van Microsoft gegevens wetenschappelijke VM gemaakt voordat de 1.5.0 (vóór 3 September 2016) gebruikt een iets anders mapstructuur, dan is opgegeven in de vorige tabel. 

## <a name="next-steps"></a>Volgende stappen
Hier volgen enkele volgende stappen om door te gaan van uw leren en te verkennen. 

* Kennismaken met de verschillende wetenschappelijke hulpmiddelen voor gegevens op de wetenschappelijke gegevens VM door te klikken op het startmenu en klik in het menu om de hulpmiddelen voor uit te checken weergegeven.
* Ga naar **C:\Program Files\Microsoft SQL Server\130\R_SERVER\library\RevoScaleR\demoScripts** voor voorbeelden met behulp van de bibliotheek RevoScaleR in R die ondersteuning biedt voor analyses van de gegevens bij het op schaal van de onderneming.  
* Lees het artikel: [10 zaken die u in de wetenschappelijke gegevens VM doen kunt](http://aka.ms/dsvmtenthings)
* Informatie over het maken van analytical end-to-end-oplossingen systematisch met behulp van het [Team gegevens wetenschap proces](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).
* Ga naar de [Galerie met Cortana bedrijfsinformatie](http://gallery.cortanaintelligence.com) voor machine learning en gegevens analytics voorbeelden die de Cortana Intelligence-Suite gebruiken. We hebben ook een pictogram opgegeven in het menu **Start** en klik op het bureaublad van de virtuele machine aan deze galerie.

