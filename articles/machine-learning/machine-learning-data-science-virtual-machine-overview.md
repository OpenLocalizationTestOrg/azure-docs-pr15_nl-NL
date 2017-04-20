<properties
    pageTitle="Wat is een gegevens wetenschappelijke virtuele Machine? | Microsoft Azure"
    description="Leer belangrijke scenario's, functies, en hoe u aan de slag met gegevens wetenschappelijke virtuele Machines, een omgeving en de toolkit klaar van analysegegevens."
    keywords="hulpmiddelen voor gegevens wetenschappelijke, gegevens wetenschappelijke virtuele machine, hulpmiddelen voor gegevens wetenschap, linux gegevens wetenschappelijk"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="bradsev" />


# <a name="introduction-to-the-cloud-based-data-science-virtual-machine-for-linux-and-windows"></a>Inleiding naar de cloud gebaseerde gegevens wetenschappelijke virtuele Machine voor Linux en Windows

De gegevens wetenschappelijke virtuele Machine is de afbeelding van een aangepaste VM op van Microsoft Azure cloud die speciaal voor gegevens wetenschappelijke doen. Er is veel populaire gegevens wetenschap en andere hulpprogramma's voor het vooraf geïnstalleerd en geconfigureerd voor vooraf snel bouwen intelligente toepassingen voor geavanceerde analyses. Dit is beschikbaar op Windows Server 2012 of Linux OpenLogic 7.2 CentOS gebaseerde versies. 

In dit onderwerp wordt uitgelegd wat u kunt doen met de gegevens wetenschappelijke VM, worden enkele belangrijke scenario's voor het gebruik van de VM itemizes van de belangrijkste functies die beschikbaar zijn op de versies van Windows en Linux en bevat instructies voor het aan de slag met deze.


## <a name="what-can-i-do-with-the-data-science-virtual-machine"></a>Wat kan ik doen met de gegevens wetenschappelijke virtuele Machine?

Het doel van de gegevens wetenschappelijke Virtual Machine is gegevens professionals op alle niveaus van de vaardigheden en rollen voorzien van een wrijving gegevens wetenschappelijke-omgeving. Deze VM bespaart u veel tijd dat u besteden wilt als u een vergelijkbare-omgeving op uw eigen had geïmplementeerd. In plaats daarvan uw gegevens wetenschappelijk project direct gestart in een nieuw gemaakte VM exemplaar. 

De gegevens wetenschappelijke VM is ontworpen en geconfigureerd voor het werken met een globaal gebruik scenario's. U kunt uw omgeving schalen omhoog of omlaag als uw project aanbrengen wilt. U bent gebruikmaken van de voorkeurstaal aan programma gegevens wetenschappelijke taken. U kunt andere hulpprogramma's installeert en het systeem voor uw exacte behoeften aanpassen.
 
## <a name="key-scenarios"></a>Belangrijke scenario 's
Deze sectie bevat enkele belangrijke scenario's waarvoor de gegevens wetenschappelijke VM kan worden geïmplementeerd.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Vooraf geconfigureerde analytics bureaublad in de cloud

De gegevens wetenschappelijke VM biedt een basislijn-configuratie voor gegevens wetenschappelijke teams wilt hun lokale bureaublad vervangen door een beheerde cloud-bureaublad. Deze basislijn zorgt ervoor dat alle partij gegevens in een team is nu een consistente instellen waarmee u bevestigt experimenten en samenwerking promoveren hebben. Ook verlaagt de kosten door de systeembeheerder belasting en klik op de tijd die nodig zijn om te beoordelen, installeren en voor het behoud van de verschillende softwarepakketten geavanceerde analyses hoeft op te slaan.  

### <a name="data-science-training-and-education"></a>Gegevens wetenschappelijke training en onderwijs

Enterprise opleiders en docenten leren die gegevens wetenschappelijke klassen bevatten meestal de afbeelding van een virtuele machine om ervoor te zorgen dat hun leerlingen/studenten een consistente installatie en dat de voorbeelden volgens plan werken. De gegevens wetenschappelijke VM Hiermee maakt u een op aanvraag-omgeving met een consistente setup die de uitdagingen incompatibiliteit tussen en ondersteuning vergemakkelijkt. Gevallen waarin deze omgevingen vaak, met name voor korter cursussen moeten, worden opgebouwd profiteren aanzienlijk.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Elastische capaciteit op aanvraag voor grootschalige projecten

Gegevens wetenschappelijke hackathons/wedstrijden of grootschalige gegevensmodellering en te verkennen vereisen schaal out hardwarecapaciteit, meestal voor de korte duur. De gegevens wetenschappelijke VM kunt repliceren van de omgeving van de wetenschappelijke gegevens snel op aanvraag, op schaal out servers waarmee experimenten vereisen van krachtige bronnen moet worden uitgevoerd.

### <a name="short-term-experimentation-and-evaluation"></a>Korte experimenten en evaluatie

De gegevens wetenschappelijke VM kan worden gebruikt om te beoordelen of informatie over hulpprogramma's zoals Microsoft R-Server, SQL Server, Visual Studio tools, Jupyter, diepblauwe leermateriaal / ML toolkits en nieuwe hulpmiddelen populaire in de community met minimale setup hoeveelheid. Aangezien de gegevens wetenschappelijke VM kan snel worden ingesteld, kan deze worden toegepast in andere korte termijn gebruik scenario's zoals repliceren gepubliceerde experimenten, demo's, volgende walkthroughs in sessies met online- of telefonische vergadering zelfstudies uitvoeren.


## <a name="whats-included-in-the-data-science-vm"></a>Wat inbegrepen in de gegevens wetenschappelijke VM?

De gegevens wetenschappelijke virtuele Machine heeft vele populaire wetenschappelijke hulpmiddelen voor gegevens al zijn geïnstalleerd en geconfigureerd. Het bevat ook hulpprogramma's waarmee u eenvoudig kunt werken met verschillende Azure-gegevens en analyses producten. U kunt verkennen en kunt combineren blog modellen grootschalige gegevenssets de Microsoft-R-Server of SQL Server-2016 gebruiken. Een groot aantal andere hulpprogramma's van de community bron openen en naar Microsoft worden ook zijn opgenomen, evenals voorbeeld code en notitieblokken. De volgende tabel itemizes en verschilt van de belangrijkste onderdelen die deel uitmaken van de Windows en Linux-edities van de gegevens wetenschappelijke Virtual Machine.


|**Windows-editie** | **Linux-editie** |
|----------------|---------------|
|Microsoft R Server Developer Edition | Microsoft R Server Developer Edition |
|Anaconda Python 2.7, 3.5 | Anaconda Python 2.7, 3.5 |
|Jupyter notitieblok Server (R, Python) | JupyterHub: Meerdere gebruikers Jupyter notitieblokken (R, Python, Julia) |
|SQL Server 2016 Developer Edition: Scalable in de database analyses met R-services | Postgres, eekhoorn SQL (databasehulpmiddel), SQL Server-stuurprogramma's en opdrachtregel (bcp, sqlcmd) |
|Visual Studio-Community-editie 2015 (IDE) </br> -Azure HDInsight (Hadoop), gegevens Lake, SQL Server Data tools </br> -Node.js, Python en R tools voor Visual Studio |IDEs en editors </br> -Eclips met de invoegtoepassing voor Azure Toolkit gebruiken </br> -Emacs (met ESS, auctex) gedit |
|Power BI desktop | -- |
|Learning gereedschapsmachines </br> -Integratie met Azure Machine Learning </br> -CNTK (uitgebreide learning/AI) </br> -Xgboost (populaire ML hulpprogramma in gegevens wetenschappelijke wedstrijden) </br> -Vowpal Wabbit (snelle online ingesteld) </br> -Rattle (visuele gegevens van de werkbalk Snel starten en analyses hulpmiddel) </br> -Mxnet (uitgebreide learning/AI) | Learning gereedschapsmachines </br> -Integraties met Azure Machine Learning </br> -CNTK (uitgebreide learning/AI) </br> -Xgboost (populaire ML hulpprogramma in gegevens wetenschappelijke wedstrijden) </br> -Vowpal Wabbit (snelle online ingesteld) </br> -Rattle (visuele gegevens van de werkbalk Snel starten en analyses hulpmiddel)  |
| SDK's voor toegang tot Azure en bedrijfsinformatie-Suite Cortana van services | SDK's voor toegang tot Azure en bedrijfsinformatie-Suite Cortana van services |
| Hulpmiddelen voor het verplaatsen van gegevens en het beheer van Azure en Big Data resources: Azure opslag Explorer, CLI, PowerShell, AdlCopy (Azure gegevens Lake), AzCopy, dtui (voor DocumentDB), Microsoft Data Management Gateway | Hulpmiddelen voor het verplaatsen van gegevens en het beheer van Azure en Big Data resources: Azure opslag Explorer, CLI |
| Cijfer, Visual Studio Team Services-invoegtoepassing | Cijfer |
| Windows-poort van de populairste Linux/Unix hulpprogramma toegankelijk is via GitBash/opdrachtprompt | -- |



## <a name="how-to-get-started-with-the-windows-data-science-vm"></a>Hoe u aan de slag met de Windows gegevens wetenschappelijke VM

- Maak een exemplaar van de VM op Windows door te navigeren naar [deze pagina](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) en de knop met groen **VM maken** te selecteren.
- Meld u aan bij de VM vanuit uw extern bureaublad met de referenties die u hebt opgegeven toen u de VM hebt gemaakt.
- Als u wilt ontdekken en de hulpprogramma's te starten, klikt u op het menu **Start** .


## <a name="get-started-with-the-linux-data-science-vm"></a>Aan de slag met de Linux gegevens wetenschappelijke VM

- Maak een exemplaar van de VM op Linux (OpenLogic CentOS-basis) door te navigeren naar [deze pagina](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/) en de knop **VM maken** te selecteren.
- Meld u aan bij de VM in een SSH-client zoals stopverf of SSH opdracht, met de referenties die u hebt opgegeven wanneer u de VM hebt gemaakt.
- Voer in de prompt shell dsvm-meer-info.
- Voor een grafische bureaublad, de X2Go-client voor uw platform client downloaden [hier](http://wiki.x2go.org/doku.php/doc:installation:x2goclient) en volg de instructies in het document Linux gegevens wetenschappelijke VM [inrichten de Linux gegevens wetenschappelijke virtuele Machine](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).


## <a name="next-steps"></a>Volgende stappen

### <a name="for-the-windows-data-science-vm"></a>Voor de Windows gegevens wetenschappelijke VM

- Zie voor meer informatie over het uitvoeren van bepaalde hulpprogramma's op de Windows-versie, [inrichten Microsoft gegevens wetenschappelijke VM](machine-learning-data-science-provision-vm.md) en
-  Zie voor meer informatie over hoe u verschillende taken die u nodig hebt voor uw gegevens wetenschappelijk project op de Windows-VM uitvoert, [tien wat die u in de wetenschappelijke gegevens VM doen kunt](machine-learning-data-science-vm-do-ten-things.md).

### <a name="for-the-linux-data-science-vm"></a>Voor de wetenschappelijke Linux gegevens VM

- Zie voor meer informatie over het uitvoeren van bepaalde hulpprogramma's op de versie Linux [inrichten de Linux gegevens wetenschappelijke virtuele Machine](machine-learning-data-science-linux-dsvm-intro.md).
- Zie voor stapsgewijze instructies die u hoe ziet u verschillende algemene gegevens wetenschappelijke taken met de Linux VM uitvoeren, [wetenschappelijke gegevens op de Linux wetenschappelijke virtuele Machine gegevens](machine-learning-data-science-linux-dsvm-walkthrough.md).
