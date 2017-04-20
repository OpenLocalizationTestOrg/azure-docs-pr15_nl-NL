<properties
   pageTitle="Migreren van Orchestrator naar Azure automatisering | Microsoft Azure"
   description="Beschrijving van het systeem Center Orchestrator runbooks en integratie packs migreren naar Azure automatisering."
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="bwren" />


# <a name="migrating-from-orchestrator-to-azure-automation-beta"></a>Migreren van Orchestrator naar Azure automatisering (Beta)

Runbooks in [Systeem Center Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) zijn gebaseerd op de activiteiten van integratie packs die specifiek voor Orchestrator zijn geschreven terwijl runbooks in Azure automatisering zijn gebaseerd op de Windows PowerShell.  [Grafische runbooks](automation-runbook-types.md#graphical-runbooks) in Azure automatisering hebben een vergelijkbare weergave aan Orchestrator runbooks met hun activiteiten die PowerShell-cmdlets, onderliggende runbooks en activa vertegenwoordigt.

De [Systeem Center Orchestrator migratie Toolkit](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) bevat hulpmiddelen voor u helpen bij het converteren van runbooks van Orchestrator naar Azure automatisering.  Naast het converteren van de runbooks zelf, moet u de packs integratie met de activiteiten die gebruikmaken van de runbooks converteren naar integratiemodules met Windows PowerShell-cmdlets.  

Hieronder volgt de basisbeginselen voor het converteren van Orchestrator runbooks naar Azure automatisering.  Elk van deze stappen wordt beschreven in detail in de onderstaande secties.

1.  Download de [Systeem Center Orchestrator migratie Toolkit](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) waarin de hulpprogramma's en modules in dit artikel beschreven.
2.  [Standaard activiteiten Module](#standard-activities-module) in Azure automatisering importeren.  Dit geldt ook voor geconverteerde versies van standaard Orchestrator activiteiten die kunnen worden gebruikt door de geconverteerde runbooks.
3.  [Systeem Center Orchestrator integratiemodules](#system-center-orchestrator-integration-modules) importeren in Azure automatisering voor deze integratie packs die worden gebruikt door uw runbooks die toegang System Center tot.
4.  Aangepaste en derden packs voor integratie met het [Conversieprogramma voor integratie Pack](#integration-pack-converter) converteren en importeren in Azure automatisering.
5.  Orchestrator runbooks met het [Runbook conversieprogramma](#runbook-converter) converteren en installeer in Azure automatisering.
6.  Handmatig vereiste Orchestrator activa maken in Azure automatisering, aangezien het conversieprogramma Runbook wordt niet geconverteerd deze resources.
7.  Een [Hybride Runbook werknemer](#hybrid-runbook-worker) in uw lokale datacenter om uit te voeren geconverteerde runbooks die toegang lokale bronnen tot configureren.

## <a name="service-management-automation"></a>Service Management automatisering

[Service Management automatisering](http://technet.microsoft.com/library/dn469260.aspx) (SMA) worden opgeslagen en wordt runbooks in uw lokale Datacenter zoals Orchestrator uitgevoerd en wordt de dezelfde integratiemodules als Azure automatisering gebruikt. Het [Runbook conversieprogramma](#runbook-converter) converteert Orchestrator runbooks naar grafische runbooks maar die niet worden ondersteund in SMA.  U kunt nog steeds de [Standaardmodule activiteiten](#standard-activities-module) en [Systeem Center Orchestrator integratiemodules](#system-center-orchestrator-integration-modules) in SMA installeren, maar moet u handmatig [herschreven fragment uw runbooks](http://technet.microsoft.com/library/dn469262.aspx).

## <a name="hybrid-runbook-worker"></a>Hybride Runbook werknemer

Runbooks in Orchestrator zijn opgeslagen op een databaseserver en worden uitgevoerd op runbook servers, zowel in uw lokale Datacenter.  Runbooks in Azure automatisering worden opgeslagen in de cloud Azure en kan worden uitgevoerd in uw lokale datacenter met een [Hybride Runbook werknemer](automation-hybrid-runbook-worker.md).  Dit is hoe u meestal runbooks met Orchestrator geconverteerd, omdat ze zijn ontworpen om uit te voeren op lokale servers wordt uitgevoerd.

## <a name="integration-pack-converter"></a>Integratie Pack conversieprogramma

Integratie Pack conversieprogramma converteert integratie packs die zijn gemaakt met de [Orchestrator integratie Toolkit (OIT)](http://technet.microsoft.com/library/hh855853.aspx) naar integratiemodules op basis van Windows PowerShell die kunnen worden geïmporteerd in Azure automatisering of Service Management automatisering.  

Wanneer u het conversieprogramma voor integratie Pack uitvoert, wordt er weergegeven met een wizard waarmee u een integratie pack (.oip)-bestand te selecteren.  De wizard vervolgens lijsten van de activiteiten die zijn opgenomen in dat integratie-pakket en kunt u selecteren die worden gemigreerd.  Wanneer u de wizard hebt voltooid, wordt een integratiemodule dat een bijbehorende cmdlet voor elk van de activiteiten in het oorspronkelijke integratie taalpakket bevat.


### <a name="parameters"></a>Parameters

De eigenschappen van een activiteit in het taalpakket integratie worden geconverteerd naar parameters van de corresponderende cmdlet in de integratiemodule.  Windows PowerShell-cmdlets hebben een set [algemene parameters](http://technet.microsoft.com/library/hh847884.aspx) die kunnen worden gebruikt met alle cmdlets.  Bijvoorbeeld de uitgebreide parameter - Hiermee wordt een cmdlet om gedetailleerde informatie over de werking.  Geen cmdlet wellicht een parameter met dezelfde naam als een algemene parameter.  Als een activiteit een eigenschap met dezelfde naam als een algemene parameter heeft, vraagt de wizard u een andere naam voor de parameter weer te geven.

### <a name="monitor-activities"></a>Monitor activiteiten

Monitor runbooks in Orchestrator starten met een [activiteiten controleren](http://technet.microsoft.com/library/hh403827.aspx) en uitvoeren continu wachten moet worden gestart door een bepaalde gebeurtenis.  Azure-automatisering ondersteunt geen monitor runbooks, zodat een monitor activiteiten in het taalpakket integratie niet worden geconverteerd.  In plaats daarvan is een tijdelijke aanduiding voor cmdlet gemaakt in de integratiemodule voor de activiteit monitor.  Deze cmdlet heeft geen functionaliteit, maar u kunt een geconverteerde runbook waarin deze zijn geïnstalleerd.  Deze runbook worden niet uitgevoerd in Azure automatisering, maar deze kan worden geïnstalleerd zodat u deze kunt wijzigen.

### <a name="integration-packs-that-cannot-be-converted"></a>Integratie packs kunnen niet worden geconverteerd

Integratie packs die niet zijn gemaakt met OIT kunnen niet worden geconverteerd met het conversieprogramma voor integratie Pack. Er zijn ook enkele integratie packs geleverd door Microsoft die momenteel kan niet worden geconverteerd met dit hulpprogramma.  Geconverteerde versies van deze packs integratie zijn [opgegeven voor downloaden](#system-center-orchestrator-integration-modules) zodat ze in Azure automatisering of Service Management automatisering kunnen worden geïnstalleerd.


## <a name="standard-activities-module"></a>Standaard activiteiten Module

Orchestrator bevat een [standaard activiteiten](http://technet.microsoft.com/library/hh403832.aspx) die niet zijn opgenomen in een taalpakket integratie, maar worden gebruikt door veel runbooks.  De module standaard activiteiten is een integratiemodule met een equivalente cmdlet voor elk van deze activiteiten.  Voordat u een geconverteerde runbooks die gebruikmaken van de activiteit in een standaard importeert, moet u deze integratiemodule installeren in Azure automatisering.

Naast het ondersteunende geconverteerde runbooks, kan de cmdlets in de module standaard activiteiten worden gebruikt door iemand vertrouwd met Orchestrator maken van nieuwe runbooks in Azure automatisering.  Terwijl de functionaliteit van alle standaard activiteiten kan worden uitgevoerd met cmdlets, kunnen ze anders werken.  De cmdlets in de module geconverteerde standaard activiteiten worden werken op dezelfde manier als hun bijbehorende activiteiten en dezelfde parameters gebruiken.  Hiermee kunt de bestaande Orchestrator runbook auteur in hun overgang naar Azure automatisering runbooks.

## <a name="system-center-orchestrator-integration-modules"></a>System Center Orchestrator integratiemodules

Microsoft biedt [integratie packs](http://technet.microsoft.com/library/hh295851.aspx) voor het samenstellen van runbooks wilt automatiseren System Center onderdelen en andere producten.  Sommige van deze integratie packs momenteel zijn gebaseerd op OIT maar momenteel niet worden omgezet in integratiemodules vanwege bekende problemen.  [Systeem Center Orchestrator integratiemodules](https://www.microsoft.com/download/details.aspx?id=49555) bevat geconverteerde versies van deze integratie packs die kunnen worden geïmporteerd in Azure automatisering en Service Management automatisering.  

Door de RTM-versie van dit hulpprogramma worden bijgewerkte versies van de integratie packs op basis van OIT die kan worden omgezet met het conversieprogramma voor integratie Pack gepubliceerd.  Richtlijnen ook krijgt u helpen bij het converteren van runbooks via de activiteiten in de integratie packs niet is gebaseerd op OIT.

## <a name="runbook-converter"></a>Runbook conversieprogramma

Orchestrator runbooks converteert Runbook conversieprogramma naar [grafische runbooks](automation-runbook-types.md#graph-runbooks) die kunnen worden geïmporteerd in Azure automatisering.  

Runbook conversieprogramma wordt geïmplementeerd als een PowerShell-module met een cmdlet **ConverterenVan-SCORunbook** waarmee de conversie genoemd.  Wanneer u het hulpmiddel installeert, wordt er een snelkoppeling naar een PowerShell-sessie waarmee wordt geladen de cmdlet gemaakt.   

Hieronder volgt de basic-proces voor het converteren van een runbook Orchestrator en importeren in Azure automatisering.  De volgende gedeelten wordt nader verder details over het gebruik van het hulpmiddel en werken met geconverteerde runbooks.

1. Een of meer runbooks uit Orchestrator exporteren.
2. Van integratiemodules voor alle activiteiten in het runbook aanvragen.
3. De runbooks Orchestrator in het geëxporteerde bestand converteren.
4. Controleer de gegevens in de logboeken voor het valideren van de conversie en om te bepalen de vereiste handmatige taken.
4. Geconverteerde runbooks in Azure automatisering importeren.
5. Maak alle vereiste activa in Azure automatisering.
6. Bewerk het runbook in Azure automatisering voor het wijzigen van alle vereiste activiteiten.

### <a name="using-runbook-converter"></a>Met Runbook programma te converteren

De syntaxis voor **ConverterenVan-SCORunbook** is als volgt:

    ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string> 

- RunbookPath - pad naar het exportbestand met de runbooks te converteren.
- Module - door komma's gescheiden lijst met integratiemodules met activiteiten in de runbooks.
- OutputFolder - pad naar de map maken geconverteerd grafische runbooks. 


De volgende opdracht converteert de runbooks in een exportbestand **MyRunbooks.ois_export**genoemd.  Deze runbooks gebruikt u de integratie van Active Directory en Data Protection Manager packs.

    ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks" 


### <a name="log-files"></a>Logboekbestanden

Het conversieprogramma Runbook maakt de volgende logboekbestanden op dezelfde locatie als het geconverteerde runbook.  Als de bestanden die al bestaat, wordt deze overschreven met de informatie van de laatste conversie.

| Bestand | Inhoud |
|:---|:---|
| Runbook conversieprogramma - Progress.log | Gedetailleerde stappen van de conversie met inbegrip van informatie voor elke activiteit heeft geconverteerd en waarschuwing voor elke activiteit niet geconverteerd. |
| Runbook conversieprogramma - Summary.log  | Overzicht van de laatste conversie, inclusief eventuele waarschuwingen en taken die u nodig hebt om uit te voeren zoals het maken van een variabele die is vereist voor het geconverteerde runbook opvolgen.  |

### <a name="exporting-runbooks-from-orchestrator"></a>Runbooks exporteren uit Orchestrator

Het conversieprogramma Runbook werkt met een bestand exporteren uit Orchestrator die een of meer runbooks bevat.  Deze wilt een bijbehorende Azure automatisering runbook voor elke runbook Orchestrator tijdens het exportbestand maken.  

Als u wilt exporteren een runbook uit Orchestrator, met de rechtermuisknop op de naam van het runbook in Runbook Designer en selecteer **exporteren**.  Als u wilt exporteren alle runbooks in een map, met de rechtermuisknop op de naam van de map en selecteer **exporteren**.


### <a name="runbook-activities"></a>Runbook activiteiten

Elke activiteit in het runbook Orchestrator converteert Runbook conversieprogramma naar een overeenkomstige activiteit in Azure automatisering.  Voor de activiteiten die kunnen niet worden geconverteerd, de activiteit in een tijdelijke aanduiding voor gemaakt in het runbook met waarschuwingstekst.  Nadat u het geconverteerde runbook in Azure automatisering importeert, kunt u een van deze activiteiten moet vervangen door geldige activiteiten die de vereiste functionaliteit uitvoeren.

Orchestrator activiteiten in de [Standaardmodule activiteiten](#standard-activities-module) worden geconverteerd.  Er zijn enkele standaard Orchestrator activiteiten die zich niet in deze module wel en niet worden omgezet.  **Verzenden platformgebeurtenis** heeft bijvoorbeeld geen equivalent Azure-automatisering sinds de gebeurtenis bij Orchestrator hoort.

[Monitor activiteiten](https://technet.microsoft.com/library/hh403827.aspx) worden niet geconverteerd omdat er geen equivalent ze in Azure automatisering.  De uitzondering zijn monitor activiteiten in [geconverteerd integratie packs](#integration-pack-converter) die wordt geconverteerd naar de tijdelijke aanduiding voor activiteit.

Een activiteit van een [integratie pack geconverteerd](#integration-pack-converter) wordt, geconverteerd als u het pad naar de integratiemodule met de parameter **modules** .  Voor systeem Center integratie Packs, kunt u het [Systeem Center Orchestrator integratiemodules](#system-center-orchestrator-integration-modules).


### <a name="orchestrator-resources"></a>Orchestrator resources

Het conversieprogramma Runbook converteert alleen runbooks, geen andere Orchestrator resources zoals items, variabelen of verbindingen.  Items worden niet ondersteund in Azure automatisering.  Variabelen en verbindingen worden ondersteund, maar u moet deze handmatig maken.  De logboekbestanden worden gemeld als het runbook vereist is voor deze middelen en geef corresponderende bronnen die u nodig hebt om in Azure automatisering voor het geconverteerde runbook werking te maken.

Een runbook kunt bijvoorbeeld een variabele naar een bepaalde waarde in een activiteit te vullen.  Het geconverteerde runbook wordt de activiteit converteren en geeft u een variabele activa in Azure automatisering met dezelfde naam als de variabele Orchestrator.  Dit zijn vastgelegd in het **Runbook conversieprogramma - Summary.log** -bestand dat is gemaakt na de conversie.  U moet deze variabele activa handmatig maken in Azure automatisering vóór het gebruik van het runbook. 

 
### <a name="input-parameters"></a>Invoerparameters

Runbooks in Orchestrator accepteren invoerparameters met de **Gegevens initialisatie** activiteit.  Als het runbook wordt geconverteerd deze activiteit bevat, wordt klikt u vervolgens een [invoerparameter](automation-graphical-authoring-intro.md#runbook-input-and-output) in het runbook Azure automatisering gemaakt voor elke parameter in de activiteit.  Een activiteit in een [besturingselement werkstroom Script](automation-graphical-authoring-intro.md#activities) is gemaakt in het geconverteerde runbook die zijn opgehaald en elke parameter geeft.  Alle activiteiten in het runbook die gebruikmaken van een invoerparameter verwijzen naar de uitvoer van deze activiteit.

De reden dat deze strategie wordt gebruikt, is het beste gespiegelde de functionaliteit in het runbook Orchestrator.  Activiteiten in nieuwe grafische runbooks moeten verwijzen naar rechtstreeks met behulp van een bron van de invoergegevens Runbook invoerparameters.


### <a name="invoke-runbook-activity"></a>Roepen Runbook activiteit

Andere runbooks beginnen Runbooks in Orchestrator met de activiteit **Runbook roepen** . Als het runbook wordt geconverteerd deze activiteit bevat en de optie **Wachten voltooid** is ingesteld, klikt u vervolgens een runbook activiteit wordt gemaakt voor deze in het geconverteerde runbook.  Als de optie **wachten voltooiing** niet is ingesteld, is klikt u vervolgens de activiteit in een werkstroom voor het Script gemaakt waarin **Start-AzureAutomationRunbook** voor het starten van het runbook wordt gebruikt.  Nadat u het geconverteerde runbook in Azure automatisering importeert, moet u deze activiteit wijzigen met de gegevens die zijn opgegeven in de activiteit.




## <a name="related-articles"></a>Verwante artikelen

- [Systeem Center 2012 - Orchestrator](http://technet.microsoft.com/library/hh237242.aspx)
- [Service Management automatisering](https://technet.microsoft.com/library/dn469260.aspx)
- [Hybride Runbook werknemer](automation-hybrid-runbook-worker.md)
- [Standaard activiteiten orchestrator](http://technet.microsoft.com/library/hh403832.aspx)
- [Systeem Downloadcentrum Orchestrator migratie Toolkit gebruiken](https://www.microsoft.com/en-us/download/details.aspx?id=47323)
 
