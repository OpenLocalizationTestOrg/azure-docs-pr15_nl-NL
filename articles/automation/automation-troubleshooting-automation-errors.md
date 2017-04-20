<properties
   pageTitle="Azure automatisering foutafhandeling | Microsoft Azure"
   description="In dit artikel biedt eenvoudige fout afhandeling stappen om oplossen en oplossen van veelvoorkomende fouten in Azure automatisering."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="stevenka"
   editor="tysonn"
   tags="top-support-issue"
   keywords="automatisering is opgetreden en foutafhandeling"/>
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/06/2016"
   ms.author="sngun; v-reagie"/>

# <a name="error-handling-tips-for-common-azure-automation-errors"></a>Tips voor het veelvoorkomende fouten in Azure automatisering voor foutverwerking

In dit artikel wordt uitgelegd enkele van de algemene Azure automatisering fouten ziet u mogelijk ervaart en stappen worden voorgesteld mogelijke stappen voor foutafhandeling.

## <a name="troubleshoot-authentication-errors-when-working-with-azure-automation-runbooks"></a>Verificatie fouten tijdens het werken met Azure automatisering runbooks  

### <a name="scenario-sign-in-to-azure-account-failed"></a>Scenario: Meld u aan bij Azure-Account is mislukt

**Fout:** Het foutbericht "Unknown_user_type: onbekend gebruikerstype" tijdens het werken met de cmdlets toevoegen-AzureAccount of aanmeldings-AzureRmAccount.

**Reden voor de fout:** Deze fout treedt op als de naam van de activa referentie ongeldig is of als de gebruikersnaam en het wachtwoord dat u gebruikt voor het instellen van de activa van de referentie automatisering niet geldig zijn.

**Tips voor probleemoplossing:** Om te bepalen wat is er mis, voert u de volgende stappen uit:  

1. Zorg ervoor dat er geen speciale tekens bevat, inclusief de **@** teken in de automatisering referentie Activumnaam waarmee u verbinding maakt met Azure werkt.  

2. Controleer waarmee u kunt de gebruikersnaam en wachtwoord die zijn opgeslagen in de referentie Azure automatisering in uw lokale PowerShell ISE-editor. U kunt dit doen door de volgende cmdlets uit te voeren in de PowerShell ISE:  

        $Cred = Get-Credential  
        #Using Azure Service Management   
        Add-AzureAccount –Credential $Cred  
        #Using Azure Resource Manager  
        Login-AzureRmAccount –Credential $Cred

3. Als de verificatie lokaal, dit betekent dat u uw Azure Active Directory-referenties juist nog niet hebt ingesteld mislukt. Raadpleeg [Authenticating naar Azure met Azure Active Directory](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) -blogbericht om de Azure Active Directory-account correct ingesteld.  


### <a name="scenario-unable-to-find-the-azure-subscription"></a>Scenario: Niet vinden van de Azure-abonnement

**Fout:** Het foutbericht ' het abonnement met de naam ``<subscription name>`` is niet gevonden "tijdens het werken met de cmdlets Selecteer-AzureSubscription of selecteer-AzureRmSubscription.

**Reden voor de fout:** Deze fout treedt op als de naam van het abonnement ongeldig is of als de Azure Active Directory-gebruiker die probeert te krijgen van de details van abonnement niet is geconfigureerd als beheerder van het abonnement.

**Tips voor probleemoplossing:** Om te bepalen als u hebt geverifieerd naar Azure en toegang hebt tot het abonnement dat u wilt selecteren, voert u de volgende stappen uit:  

1. Zorg ervoor dat u de **Toevoegen-AzureAccount** worden uitgevoerd voordat u de cmdlet **Selecteer-AzureSubscription** wordt uitgevoerd.  

2. Als u dit foutbericht nog steeds ziet, wordt uw code wijzigen door het toevoegen van de cmdlet **Get-AzureSubscription** is de cmdlet **Toevoegen-AzureAccount** volgen en klikt u vervolgens de code uitvoeren.  Controleer nu of als de uitvoer van Get-AzureSubscription de details van uw abonnement bevat.  
    * Als u de details van een abonnement in de uitvoer niet ziet, betekent dit dat het abonnement dat nog niet is geïnitialiseerd.  
    * Als u de details van abonnement in de uitvoer ziet, moet u ervoor zorgen dat u de abonnementsnaam van de juiste of ID met de cmdlet **Selecteer-AzureSubscription** .   


### <a name="scenario-authentication-to-azure-failed-because-multi-factor-authentication-is-enabled"></a>Scenario: Verificatie naar Azure is mislukt omdat meervoudige verificatie is ingeschakeld

**Fout:** Het foutbericht "toevoegen-AzureAccount: AADSTS50079: sterke verificatie registratie (bewijs boven) is vereist ' bij het verifiëren van Azure met uw Azure gebruikersnaam en wachtwoord.

**Reden voor de fout:** Als u meervoudige verificatie van uw Azure-account hebt, kunt u een Azure Active Directory-gebruiker niet gebruiken om te verifiëren op Azure.  In plaats daarvan, moet u een certificaat of een service principal gebruiken om te verifiëren bij Azure.

**Tips voor probleemoplossing:** Als u wilt een certificaat gebruikt met de cmdlets voor het beheer van Azure-Service, raadpleegt u [maken en het toevoegen van een certificaat voor het beheren van Azure services.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Als u wilt een service principal met Azure resourcemanager cmdlets gebruiken, raadpleegt u voor het [maken van service principal met behulp van Azure portal](./resource-group-create-service-principal-portal.md) en [verifiëren van een service principal met Azure resourcemanager.](./resource-group-authenticate-service-principal.md)


## <a name="troubleshoot-common-errors-when-working-with-runbooks"></a>Oplossen van veelvoorkomende fouten tijdens het werken met runbooks

### <a name="scenario-runbook-fails-because-of-deserialized-object"></a>Scenario: Runbook mislukt vanwege gedeserialiseerde object

**Fout:** Uw runbook mislukt met de fout ' kan geen binding maken parameter ``<ParameterName>``. Kan niet worden geconverteerd de ``<ParameterType>`` waarde van het type Deserialized ``<ParameterType>`` typt ``<ParameterType>``".

**Reden voor de fout:** Als uw runbook een PowerShell-werkstroom is, slaat het complexe objecten in een gedeserialiseerde indeling om te kunnen de status van uw runbook worden gehandhaafd als de werkstroom is geschorst.  

**Tips voor probleemoplossing:**  
Een van de volgende drie oplossingen het probleem opgelost:

1. Als u complexe objecten uit één cmdlet naar een andere zijn pijpleidingplan, moet u deze cmdlets teruglopen in een InlineScript.  
2. De naam of de waarde die u nodig hebt doorgegeven van het complexe object in plaats van het doorgeven van het hele object.  

3. Gebruik een PowerShell-runbook in plaats van een werkstroom voor het PowerShell-runbook.  


### <a name="scenario-runbook-job-failed-because-the-allocated-quota-exceeded"></a>Scenario: Runbook taak is mislukt omdat de toegewezen limiet overschreden

**Fout:** Uw werk runbook mislukt met de fout "het quotum voor de maandelijkse totale taak runtime is bereikt voor dit abonnement".

**Reden voor de fout:** Deze fout treedt op wanneer de taakuitvoering groter is dan het 500 minuten gratis quotum voor uw account. Dit quotum is van toepassing op alle typen execution taken zoals testen van een taak, een taak beginnend bij de portal, een project uitvoert met webhooks en plannen van een taak uit te voeren met behulp van de Azure portal of in uw datacenter. Meer raadpleegt informatie over prijzen voor automatisering u [automatisering prijzen](https://azure.microsoft.com/pricing/details/automation/).

**Tips voor probleemoplossing:** Als u wilt gebruiken van meer dan 500 minuten van verwerking per maand moet u uw abonnement wijzigen van de gratis laag naar de eenvoudige laag. U kunt een upgrade uitvoeren naar de eenvoudige laag door de volgende stappen:  

1. Meld u aan bij uw Azure-abonnement  
2. Selecteer het automatisering-account dat u wilt bijwerken  
3. Klik op **Instellingen** > **prijzen laag en het gebruik** > **prijzen laag**  
4. Selecteer op het blad **kiezen uw prijzen laag** **eenvoudige**    


### <a name="scenario-cmdlet-not-recognized-when-executing-a-runbook"></a>Scenario: Cmdlet niet herkend bij het uitvoeren van een runbook

**Fout:** Uw werk runbook mislukt met de fout '``<cmdlet name>``: de term ``<cmdlet name>`` wordt niet herkend als de naam van een cmdlet, functie, script-bestand of programma. "

**Reden voor de fout:** Deze fout treedt op als de cmdlet die u in uw runbook gebruikt door de PowerShell-engine niet vinden.  Het is mogelijk dat de module met de cmdlet in het account ontbreekt, er een naamconflict door de naam van een runbook is, of de cmdlet ook in een andere module bestaat en automatisering de naam niet kunt oplossen.

**Tips voor probleemoplossing:** Een van de volgende oplossingen wordt het probleem opgelost:  

- Controleer of u de naam van de cmdlet correct hebt ingevoerd.  

- Controleer of de cmdlet bestaat in uw account automatisering en dat er conflicten zijn. Als u wilt controleren of de cmdlet aanwezig is, opent u een runbook in bewerkingsmodus en zoek naar de cmdlet die u wilt zoeken in de bibliotheek of uitvoeren **Get-opdracht ``<CommandName>`` **.  Nadat u hebt gevalideerd die de cmdlet beschikbaar voor het account is en dat er geen naamconflicten met andere cmdlets of runbooks, toe te voegen aan het tekenpapier en zorg ervoor dat u een geldige parameter instellen in uw runbook worden gebruikt.  

- Als u een naamconflict hebt en de cmdlet beschikbaar in twee verschillende modules is, kunt u deze kunt oplossen met behulp van de volledig gekwalificeerde naam voor de cmdlet. U kunt bijvoorbeeld **ModuleName\CmdletName**gebruiken.  

- Als u het runbook on-premises implementatie uitvoert in een hybride werknemergroep en zorg ervoor dat is de module/cmdlet op de computer waarop de werknemer hybride gehost geïnstalleerd.


### <a name="scenario-a-long-running-runbook-consistently-fails-with-the-exception-the-job-cannot-continue-running-because-it-was-repeatedly-evicted-from-the-same-checkpoint"></a>Scenario: Een langdurige runbook consistente mislukt met de uitzondering: "de taak kan niet worden voortgezet uitgevoerd omdat deze net zo vaak verwijdering uit het dezelfde controlepunt".

**Reden voor de fout:** Dit is inherent ontwerp gedrag vanwege de 'Fair-aandeel' cmdlets voor controle van processen binnen Azure automatisering, waarin een runbook automatisch onderbreekt als deze langer dan 3 uur wordt uitgevoerd. Het foutbericht dat wordt geretourneerd biedt echter geen in 'volgende' opties. Een runbook kan voor een aantal redenen worden uitgeschakeld. Onderbreekt opgetreden voornamelijk vanwege fouten. Bijvoorbeeld een onbekende uitzondering in een runbook, een netwerk is mislukt of vastlopen op de Runbook werknemer het runbook uitgevoerd, worden alle ertoe leiden dat het runbook geschorst en starten vanuit de laatste controlepunt wanneer wordt hervat.

**Tips voor probleemoplossing:** De beschreven oplossing om dit probleem te voorkomen is controlepunten gebruiken in een werkstroom.  Voor meer dat informatie raadpleegt u [Learning PowerShell werkstromen](automation-powershell-workflow.md#Checkpoints).  Een gedetailleerde informatie over het "Fair delen" en controlepunt vindt u in dit artikel blog [Controlepunten in Runbooks gebruiken](https://azure.microsoft.com/en-us/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/).


## <a name="troubleshoot-common-errors-when-importing-modules"></a>Oplossen van veelvoorkomende fouten bij het importeren van modules

### <a name="scenario-module-fails-to-import-or-cmdlets-cant-be-executed-after-importing"></a>Scenario: Module niet kan worden geïmporteerd of cmdlets kan niet worden uitgevoerd nadat u hebt geïmporteerd

**Fout:** Een module niet kan worden geïmporteerd of succes importeert, maar geen cmdlets worden geëxtraheerd.

**Reden voor de fout:** Er zijn enkele veelvoorkomende redenen dat een module niet goed naar Azure automatisering importeren mogelijk:  

- De structuur komt niet overeen met de structuur die het automatisering nodig heeft in.  

- De module is afhankelijk van een andere module die niet bij uw account automatisering is geïmplementeerd.  

- De module ontbreekt bijbehorende afhankelijkheden in de map.  

- De cmdlet **New-AzureRmAutomationModule** wordt gebruikt voor het uploaden van de module en kunt u het pad van de volledige opslag niet hebben of de module met behulp van de URL van een openbaar niet geladen.  

**Tips voor probleemoplossing:**  
Een van de volgende oplossingen wordt het probleem opgelost:  

- Zorg ervoor dat de module volgt op de volgende indeling:  
ModuleName.Zip **->** modulenaam of het versienummer **->** (ModuleName.psm1, ModuleName.psd1)

- Open het bestand .psd1 en Zie als de module afhankelijk is.  Als dat zo is, moet u deze modules uploaden naar het automatisering-account.  

- Zorg ervoor dat eventuele waarnaar wordt verwezen dll aanwezig zijn in de map module.  


## <a name="troubleshoot-common-errors-when-working-with-desired-state-configuration-dsc"></a>Oplossen van veelvoorkomende fouten tijdens het werken met gewenst staat configuratie (DSC)  

### <a name="scenario-node-is-in-failed-status-with-a-not-found-error"></a>Scenario: Knooppunt is in mislukte status met een fout "Niet gevonden"

**Fout:** Het knooppunt heeft een rapport met status **mislukt** en met de fout ' poging tot het ophalen van de actie van server https://``<url>``//accounts/``<account-id>``/Nodes(AgentId=``<agent-id>``)/GetDscAction mislukt omdat een geldige configuratie ``<guid>`` kan niet worden gevonden. "

**Reden voor de fout:** Deze fout treedt meestal wanneer het knooppunt is toegewezen aan een configuratienaam (bijvoorbeeld ABC) in plaats van een knooppunt configuratienaam (bijvoorbeeld ABC. WebServer).  

**Tips voor probleemoplossing:**  

- Zorg ervoor dat u het knooppunt met "naam knooppunt configuratie" en niet de 'configuratienaam' toewijst.  

- U kunt een knooppuntconfiguratie toewijzen aan een knooppunt met behulp van Azure portal of met een PowerShell-cmdlet.
    - Een knooppuntconfiguratie om aan te wijzen met behulp van Azure portal knooppunt, opent u het blad **DSC knooppunten** , selecteer een knooppunt en klikken op knop **knooppuntconfiguratie toewijzen** .  
    - Een knooppuntconfiguratie om aan te wijzen knooppunt PowerShell-cmdlet gebruiken, gebruikt u de cmdlet **Set-AzureRmAutomationDscNode**


### <a name="scenario--no-node-configurations-mof-files-were-produced-when-a-configuration-is-compiled"></a>Scenario: Geen knooppunt configuraties (MOF-bestanden) zijn geproduceerd wanneer een configuratie is gecompileerd

**Fout:** Uw DSC gecompileerd taak onderbreekt met de fout: "compilatie is voltooid, maar geen knooppunt configuratie .mofs zijn gegenereerd".

**Reden voor de fout:** Wanneer de volgende expressie die het trefwoord **knooppunt** in de configuratie DSC evalueert u $null en geen configuraties knooppunt worden gemaakt.    

**Tips voor probleemoplossing:**  
Een van de volgende oplossingen wordt het probleem opgelost:  

- Zorg ervoor dat de expressie naast het trefwoord **knooppunt** in de definitie van de configuratie niet naar $null is evalueren.  
- Als u ConfigurationData doorgeeft bij het compileren van de configuratie, zorg dat u de verwachte waarden die moeten worden de configuratie van [ConfigurationData](automation-dsc-compile.md#configurationdata)doorgeeft.


### <a name="scenario--the-dsc-node-report-becomes-stuck-in-progress-state"></a>Scenario: Het rapport van DSC knooppunt verandert hangen "Bezig" staat

**Fout:** DSC Agent uitvoer 'Geen instantie gevonden met de opgegeven eigenschapswaarden '.

**Reden voor de fout:** U uw WMF-versie hebt bijgewerkt en WMI hebt beschadigd.  

**Tips voor probleemoplossing:** Volg de instructies in het blogbericht [DSC bekende problemen en beperkingen voor](https://msdn.microsoft.com/powershell/wmf/limitation_dsc) het probleem op te lossen.


### <a name="scenario--unable-to-use-a-credential-in-a-dsc-configuration"></a>Scenario: Kan niet worden gebruikt een referentie in een DSC-configuratie

**Fout:** Uw DSC gecompileerd taak is onderbroken met de fout: "System.InvalidOperationException fout bij het verwerken van de eigenschap 'Referentie' van het type ``<some resource name>``: converteren en opslaan van een versleutelde wachtwoord als tekst zonder opmaak is toegestaan alleen als PSDscAllowPlainTextPassword is ingesteld op waar".

**Reden voor de fout:** U hebt een referentie gebruikt in een configuratie, maar u BEGINLETTERS **ConfigurationData** als u wilt **PSDscAllowPlainTextPassword** ingesteld op waar voor elke knooppuntconfiguratie niet zelf.  

**Tips voor probleemoplossing:**  
- Zorg ervoor dat in de juiste **ConfigurationData** naar **PSDscAllowPlainTextPassword** ingesteld op waar voor elke knooppuntconfiguratie die worden genoemd in de configuratie doorgeven. Raadpleeg [activa in Azure automatisering DSC](automation-dsc-compile.md#assets)voor meer informatie.


## <a name="next-steps"></a>Volgende stappen

Als u de bovenstaande stappen hebt gevolgd en extra hulp op een willekeurige plaats in dit artikel nodig hebt, kunt u het volgende doen:

- U kunt hulp krijgen van Azure-experts. Geef uw probleem aan de [MSDN Azure of stapel overloop forums.](https://azure.microsoft.com/support/forums/)

- Het bestand een incident Azure ondersteuning. Ga naar de [Azure Support-site](https://azure.microsoft.com/support/options/) en klik op **ondersteuning** onder **ondersteuning voor technische en facturering**.

- Publiceren op een aanvraag voor een Script op [Script Center](https://azure.microsoft.com/documentation/scripts/) als u naar een oplossing voor het runbook van Azure automatisering of een integratiemodule zoekt.

- Feedback verzamelen of aanvragen voor automatisering Azure op [Gebruiker Voice](https://feedback.azure.com/forums/34192--general-feedback)posten.
