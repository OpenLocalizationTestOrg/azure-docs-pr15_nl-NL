<properties
    pageTitle="Wat is Azure automatisering | Microsoft Azure"
    description="Welke waarde Azure automatisering bevat informatie en antwoorden op veelgestelde vragen zodat u aan de slag kunt maken, runbooks en Azure automatisering DSC gebruiken."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="Wat is automatisering, azure automatisering en azure automatisering voorbeelden"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article" 
    ms.date="05/10/2016"
    ms.author="magoedte;bwren"/>

# <a name="azure-automation-overview"></a>Overzicht van Azure automatisering

Microsoft Azure automatisering biedt een manier om gebruikers om de handmatige, langdurige lastige en vaak herhaalde taken die vaak worden uitgevoerd in een cloud- en enterprise-omgeving te automatiseren. Deze bespaart u tijd en de betrouwbaarheid van gewone beheertaken verbetert en zelfs ingesteld dat deze automatisch worden uitgevoerd met regelmatige tussenpozen. U kunt automatiseren processen met behulp van runbooks of beheer van de configuratie met gewenst staat configuratie automatiseren. In dit artikel wordt kort overzicht gegeven van Azure automatisering en vindt u antwoorden op enkele veelgestelde vragen. U kunt ook verwijzen naar andere artikelen in deze bibliotheek voor meer gedetailleerde informatie over de verschillende onderwerpen.


## <a name="automating-processes-with-runbooks"></a>Processen met runbooks automatiseren

Een runbook is een reeks taken die een geautomatiseerde proces in Azure automatisering uitvoeren. Is het mogelijk een eenvoudig proces zoals starten van een virtuele machine en een logboek maken, of u hebt een complexe runbook die worden gecombineerd andere kleinere runbooks als u wilt uitvoeren van een complex proces over meerdere resources of zelfs meerdere wolken en klik op premises omgevingen.  

Zoals u mogelijk hebt een bestaand handmatig proces voor een SQL-database worden afgekapt als deze maximumgrootte met meerdere stappen nadert zoals verbinding maken met de server, verbinden met de database, krijgen van de huidige omvang van database, controleren als drempel heeft overschreden afkappen en hoogte van de gebruiker. In plaats van handmatig uitvoering van deze stappen, kunt u een runbook die al deze taken als een afzonderlijk proces uitvoeren wilt. U start het runbook, geef de vereiste gegevens zoals SQL servernaam, de databasenaam van de, en e- en vervolgens gaan zitten terug terwijl het proces is voltooid. 


## <a name="what-can-runbooks-automate"></a>Wat kan runbooks automatiseren?

Runbooks in Azure automatisering zijn gebaseerd op de Windows PowerShell- of Windows PowerShell-werkstroom, zodat ze doen die PowerShell kunt uitvoeren. Als een toepassing of service een API heeft, kunt klikt u vervolgens een runbook werken met deze. Als u een PowerShell-module voor de toepassing hebt, kunt u die module laden in Azure automatisering en opnemen van deze cmdlets in uw runbook. Azure automatisering runbooks uitvoeren in de cloud Azure en toegang tot een cloud resources of een externe bronnen die kunnen worden geopend vanuit de cloud. [Hybride Runbook werknemer](automation-hybrid-runbook-worker.md)gebruikt, kunt runbooks uitvoeren in uw lokale Datacenter voor het beheren van lokale bronnen. 


## <a name="getting-runbooks-from-the-community"></a>Runbooks ophalen uit de community

De [Galerie met Runbook](automation-runbook-gallery.md#runbooks-in-runbook-gallery) bevat runbooks van Microsoft en de community dat u gebruiken kunt in uw omgeving ongewijzigd of ze voor uw eigen doeleinden aanpassen. Deze zijn ook nuttig vindt als verwijzingen naar informatie over het maken van uw eigen runbooks. U kunt zelfs een bijdrage leveren uw eigen runbooks in de galerie met waarvan u denkt dat andere gebruikers van pas kunnen komen. 


## <a name="creating-runbooks-with-azure-automation"></a>Runbooks maken met Azure automatisering 

U kunt [maken van uw eigen runbooks](automation-creating-importing-runbook.md) maken of wijzigen van runbooks vanuit de [Galerie met Runbook](http://msdn.microsoft.com/library/azure/dn781422.aspx) voor uw eigen vereisten. Er zijn drie verschillende [typen runbook](automation-runbook-types.md) die u kunt kiezen uit op basis van uw vereisten en de PowerShell-ervaring. Als u liever werk rechtstreeks met de PowerShell-code, kunt klikt u vervolgens u een [PowerShell-runbook](automation-runbook-types.md#powershell-runbooks) of [runbook PowerShell-werkstroom](automation-runbook-types.md#powershell-workflow-runbooks) die u offline of met de [tekstuele editor](http://msdn.microsoft.com/library/azure/dn879137.aspx) in de portal van Azure bewerken. Als u liever een runbook bewerken zonder te worden blootgesteld aan de onderliggende code, kunt u een [grafische runbook](automation-runbook-types.md#graphical-runbooks) met de [grafische editor](automation-graphical-authoring-intro.md) in de portal van Azure maken. 

Wilt u bekijken om te lezen? Bekijk de onderstaande video van Microsoft Ignite sessie in mei 2015. Opmerking: Hoewel de concepten en functies die worden besproken in deze video correct dat Azure automatisering veel zich zijn aangezien deze video is opgenomen, deze nu heeft een uitgebreidere gebruikersinterface in de portal van Azure en extra mogelijkheden ondersteunt.

> [AZURE.VIDEO microsoft-ignite-2015-automating-operational-and-management-tasks-using-azure-automation]


## <a name="automating-configuration-management-with-desired-state-configuration"></a>Beheer van de configuratie met gewenst staat configuratie automatiseren 

[PowerShell DSC](https://technet.microsoft.com/library/dn249912.aspx) is een management-platform waarmee u kunt beheren, te implementeren en configuratie voor fysieke hosts en virtuele machines met een declaratieve PowerShell-syntaxis afdwingen. U kunt configuraties definiëren op een centrale DSC halen Server die doeltoepassing machines automatisch kunnen ophalen en toepassen. DSC biedt een aantal PowerShell-cmdlets die u gebruiken kunt voor het beheren van configuraties en resources.  

[Azure automatisering DSC](automation-dsc-overview.md) is een oplossing op basis van cloud voor PowerShell DSC waarmee services is vereist voor enterprise-omgevingen.  U kunt uw resources DSC in Azure automatisering beheren en configuraties van toepassing op virtuele of fysieke machines die deze weer uit een DSC halen Server in de cloud Azure ophalen.  Het biedt ook reporting services die dat over belangrijke gebeurtenissen zoals informeren wanneer knooppunten van de toegewezen configuratie afwijken en wanneer een nieuwe configuratie is toegepast. 


## <a name="creating-your-own-dsc-configurations-with-azure-automation"></a>Uw eigen configuraties DSC maken met Azure automatisering

[DSC configuraties](automation-dsc-overview.md#azure-automation-dsc-terms) Geef de gewenste status van een knooppunt.  Meerdere knooppunten kunnen dezelfde configuratie om te kunnen waarborgen dat ze alle voor het behoud van een identieke status toepassen.  U kunt maken van een configuratie met de tekst editor op uw lokale computer en vervolgens te importeren in Azure automatisering waar u deze kunt compileren en deze knooppunten hebt toegepast.


## <a name="getting-modules-and-configurations"></a>Modules en configuraties ophalen 

U gaat [PowerShell modules](automation-runbook-gallery.md#modules-in-powershell-gallery) met cmdlets die u in uw runbooks en DSC configuraties vanuit de [Galerie met PowerShell gebruiken kunt](http://www.powershellgallery.com/). U kunt deze galerie van de Azure portal starten en modules rechtstreeks importeren in Azure automatisering, of u kunt downloaden en ze handmatig te importeren. U kunt de modules rechtstreeks vanuit de Azure-portal niet installeren, maar kunt u deze downloaden Installeer deze zoals u zou doen met een andere module. 


## <a name="example-practical-applications-of-azure-automation"></a>Voorbeeld van praktische toepassingen van Azure automatisering 

Hier volgen een paar voorbeelden van wat de soorten automatisering scenario's met Azure automatisering zijn. 

* Maak en kopieer virtuele machines in verschillende Azure-abonnementen. 
* Planningsbestand wordt opgehaald uit een lokale computer aan een container Azure-blobopslag. 
* Automatiseren beveiligingsfuncties zoals weigeren van een client aanvraagt wanneer een dergelijke aanval wordt aangetroffen. 
* Controleer of machines continu uitlijnen met geconfigureerde beveiligingsbeleid.
* Continue implementatie van toepassingscode beheren in de cloud en klik op de lokale infrastructuur. 
* Een Active Directory-bos in Azure wordt aangegeven voor uw testomgeving maken. 
* Een tabel in een SQL-database afkappen als DB maximale grootte nadert. 
* Op afstand bijwerken omgevingsinstellingen voor een Azure-website. 


## <a name="how-does-azure-automation-relate-to-other-automation-tools"></a>Hoe Azure automatisering koppelen aan andere hulpmiddelen automatisering?

[Service Management automatisering (SMA)](http://technet.microsoft.com/library/dn469260.aspx) is bedoeld om u te automatiseren beheertaken in de cloud privé. Dit is lokaal in uw datacenter geïnstalleerd als onderdeel van [Microsoft Azure Pack](https://www.microsoft.com/en-us/server-cloud/). SMA en Azure automatisering dezelfde runbook opmaak op basis van Windows PowerShell en de werkstroom met Windows PowerShell gebruiken, maar SMA biedt geen ondersteuning voor [grafische runbooks](automation-graphical-authoring-intro.md).  

[Systeem Center 2012 Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) is bedoeld voor lokale resources te automatiseren. Er wordt een andere runbook notatie dan Azure automatisering en Service Management automatisering gebruikt en heeft een grafische gebruikersinterface runbooks maken zonder een uitvoeren van scripts. De runbooks bestaan uit de activiteiten van integratie Packs die specifiek voor Orchestrator zijn geschreven. 


## <a name="where-can-i-get-more-information"></a>Waar vind ik meer informatie? 

Een aantal bronnen zijn beschikbaar voor u meer informatie over Azure automatisering en het maken van uw eigen runbooks. 

* **Azure automatisering bibliotheek** is waar u zich nu. De artikelen in deze bibliotheek bieden volledige documentatie bij de configuratie en beheer van Azure automatisering en voor het ontwerpen van uw eigen runbooks. 
* [Azure PowerShell-cmdlets](http://msdn.microsoft.com/library/jj156055.aspx) vindt u informatie voor het automatiseren van Azure bewerkingen via Windows PowerShell. Runbooks Gebruik deze cmdlets om te werken met Azure resources. 
* [Management Blog](https://azure.microsoft.com/blog/tag/azure-automation/) bevat de meest recente informatie op Azure automatisering en andere technologieën van Microsoft. U moet zich abonneren op deze blog voor het up-to-date houden met de meest recente uit het team Azure automatisering. 
* [Automatisering Forum](http://go.microsoft.com/fwlink/p/?LinkId=390561) kunt u vragen stellen over Azure automatisering worden verholpen door Microsoft en de automatisering-community. 
* [Azure automatisering Cmdlets](https://msdn.microsoft.com/library/mt244122.aspx) geeft informatie over het van de beheertaken automatiseren. De presentatie bevat cmdlets uit om Automation accounts, activa, runbooks, DSC beheren.


## <a name="can-i-provide-feedback"></a>Kan ik feedback geven? 

**Geef ons feedback!** Als u een Azure automatisering runbook-oplossing of een integratiemodule zoekt, boeken u de aanvraag voor een Script op Script Center. Als u feedback verzamelen of aanvragen voor Azure automatisering, zet u ze op de [Voicemail van de gebruiker](http://feedback.windowsazure.com/forums/34192--general-feedback). Bedankt. 


