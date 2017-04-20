<properties 
    pageTitle="Starten en stoppen virtuele machines met Azure automatisering - PowerShell werkstroom | Microsoft Azure"
    description="Grafische versie van Azure automatisering scenario inclusief runbooks om te starten en stoppen klassieke virtuele machines."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="07/06/2016"
    ms.author="bwren" />

# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a>Azure automatisering scenario - starten en stoppen virtuele machines

Dit scenario Azure automatisering bevat runbooks om te starten en stoppen klassieke virtuele machines.  U kunt dit scenario naar een of meer van de volgende opties:  

- Gebruik de runbooks ongewijzigd in uw eigen omgeving. 
- Wijzig de runbooks om uit te voeren aangepaste functionaliteit.  
- De runbooks bellen vanuit een ander runbook als onderdeel van een algemene oplossing. 
- Gebruik de runbooks als zelfstudies voor meer informatie over runbook authoring concepten. 

> [AZURE.SELECTOR]
- [Grafische](automation-solution-startstopvm-graphical.md)
- [PowerShell-werkstroom](automation-solution-startstopvm-psworkflow.md)

Dit is de PowerShell-werkstroom runbook-versie van dit scenario. Het is ook beschikbaar met [grafische runbooks](automation-solution-startstopvm-graphical.md).

## <a name="getting-the-scenario"></a>Het scenario ophalen

Dit scenario bestaat uit twee PowerShell werkstroom runbooks die u van de volgende koppelingen downloaden kunt.  Zie de [grafische versie](automation-solution-startstopvm-graphical.md) van dit scenario voor koppelingen naar de grafische runbooks.

| Runbook | Koppeling | Type | Beschrijving |
|:---|:---|:---|:---|
| Start-AzureVMs | [Azure klassieke VMs starten](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) | PowerShell-werkstroom | Begint u alle klassieke virtuele machines in een Azure subscriptionor alle virtuele machines met de naam van een bepaalde service. |
| Stop-AzureVMs | [Azure klassieke VMs stoppen](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) | PowerShell-werkstroom | Alle virtuele machines in een automatisering-account of alle virtuele machines met de naam van een bepaalde service stopt.  |


## <a name="installing-and-configuring-the-scenario"></a>Installeren en configureren van het scenario

### <a name="1-install-the-runbooks"></a>1. de runbooks installeren

Na het runbooks downloadt, kunt u deze met de procedure voor het importeren van [een Runbook](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook)importeren.

### <a name="2-review-the-description-and-requirements"></a>2. Bekijk de beschrijving en vereisten
De runbooks opnemen opmerkingen helptekst met een beschrijving en vereiste activa.  U kunt ook dezelfde gegevens krijgen van dit artikel. 

### <a name="3-configure-assets"></a>3. activa configureren
De runbooks vragen om de volgende elementen die u moet maken en vullen met de juiste waarden.

| Activatype | De Activumnaam van de | Beschrijving |
|:---|:---|:---|:---|
| Referentie | AzureCredential | Referenties voor een account die gemachtigd is om te starten en stoppen virtuele machines in het Azure abonnement bevat.  U kunt ook een ander referentie actief opgeven in de **referentie** -parameter van de activiteit **Toevoegen-AzureAccount** . |
| Variabele | AzureSubscriptionId | Bevat de ID van het abonnement van uw Azure-abonnement. |

## <a name="using-the-scenario"></a>Gebruik het scenario

### <a name="parameters"></a>Parameters

De runbooks hebben de volgende parameters.  U moet waarden opgeven voor parameters die verplicht en u kunt desgewenst waarden voor andere parameters afhankelijk van uw vereisten.

| Parameter | Type | Verplicht | Beschrijving |
|:---|:---|:---|:---|
| Servicenaam | tekenreeks | Nee | Als een waarde beschikbaar is, zijn klikt u vervolgens alle virtuele machines met de servicenaam van die gestart of gestopt.  Als u geen waarde beschikbaar is, zijn klikt u vervolgens alle klassieke virtuele machines in het Azure abonnement gestart of gestopt. |
| AzureSubscriptionIdAssetName | tekenreeks | Nee | Bevat de naam van de [variabele activa](#installing-and-configuring-the-scenario) waarin de abonnements-ID van uw Azure-abonnement.  Als u een waarde niet opgeeft, wordt *AzureSubscriptionId* gebruikt.  |
| AzureCredentialAssetName | tekenreeks | Nee | Bevat de naam van de [activa referentie](#installing-and-configuring-the-scenario) met de referenties voor het runbook gebruiken.  Als u een waarde niet opgeeft, wordt *AzureCredential* gebruikt.  |

### <a name="starting-the-runbooks"></a>De runbooks starten

U kunt een van de methoden bij het [starten van een runbook in Azure automatisering](automation-starting-a-runbook.md) starten een van de runbooks in dit scenario.

De volgende opdrachten in de steekproef gebruikt Windows PowerShell voor het uitvoeren van **StartAzureVMs** om alle virtuele machines beginnen met de naam van de service *MyVMService*.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a>Uitvoer

De runbooks wordt [uitvoer van een bericht](automation-runbook-output-and-messages.md) voor elke virtuele machine die aangeeft dat u al dan niet de instructie start of stop het is verzonden.  U kunt zoeken naar een bepaalde tekenreeks in de uitvoer om te bepalen van het resultaat voor elke runbook.  De mogelijke uitvoertekenreeksen worden in de volgende tabel weergegeven.

| Runbook | Voorwaarde | Bericht |
|:---|:---|:---|
| Start-AzureVMs | VM wordt al uitgevoerd  | MyVM wordt al uitgevoerd |
| Start-AzureVMs | Aanvraag starten voor VM verzonden | MyVM is gestart |
| Start-AzureVMs | Start-aanvraag voor virtuele machine is mislukt  | MyVM kan niet worden gestart |
| Stop-AzureVMs | VM is al gestopt  | MyVM is al gestopt |
| Stop-AzureVMs | Aanvraag voor virtuele machine verzonden stoppen | MyVM is gestopt |
| Stop-AzureVMs | Stop aanvraag voor VM is mislukt  | MyVM kan niet stoppen |

Bijvoorbeeld probeert het volgende codefragment uit een runbook alle virtuele machines start met de naam van de service *MyServiceName*.  Als een van de begindatum aanvragen fail, kunnen fout acties worden gemaakt. 

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action to take in case of success.
        }
        else {
            # Action to take in case of error.
        }
    }


## <a name="detailed-breakdown"></a>In detail

Hieronder volgt een gedetailleerd overzicht van de runbooks in dit scenario.  Deze informatie kunt u zojuist hebt meer hiertegen voor het ontwerpen van uw eigen automatisering scenario's of de runbooks aanpassen.

### <a name="parameters"></a>Parameters

    param (
        [Parameter(Mandatory=$false)] 
        [String]  $AzureCredentialAssetName = 'AzureCredential',
        
        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)] 
        [String] $ServiceName
    )

De werkstroom wordt gestart door de waarden voor de [Invoerparameters weergegeven](#using-the-scenario).  Standaardnamen worden gebruikt als de namen van de activa worden opgegeven.

### <a name="output"></a>Uitvoer

    # Returns strings with status messages
    [OutputType([String])]

Deze regel wordt gedeclareerd dat de uitvoer van het runbook zal een tekenreeks.  Dit is niet vereist maar het is een goede gewoonte voor wanneer het runbook wordt gebruikt als een [onderliggende runbook](automation-child-runbooks.md) zodat een bovenliggende runbook het uitvoertype weten kunnen verwachten.

### <a name="authentication"></a>Verificatie

    # Connect to Azure and select the subscription to work against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

De volgende regels Stel de [referenties](automation-configuring.md#configuring-authentication-to-azure-resources) en Azure-abonnement dat wordt gebruikt voor de rest van het runbook.
We gebruiken eerst **Get-AutomationPSCredential** om de activa waarin de referenties met toegang tot starten en stoppen virtuele machines in het Azure abonnement. Deze activa wordt **Toevoegen-AzureAccount** referenties instellen.  De uitvoer is toegewezen aan een pop-variabele, zodat deze geen deel van de uitvoer runbook uitmaakt.  

De variabele activa met de abonnements-ID met **Get-AutomationVariable** en het abonnement dat is geconfigureerd met **Selecteer AzureSubscription**opgehaald.

### <a name="get-vms"></a>VMs ophalen

    # If there is a specific cloud service, then get all VMs in the service,
    # otherwise get all VMs in the subscription.
    if ($ServiceName) 
    { 
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else 
    { 
        $VMs = Get-AzureVM
    }

**Get-AzureVM** wordt gebruikt voor het ophalen van de virtuele machines die het runbook met werkt.  Als een waarde in de **servicenaam** invoer variabele beschikbaar is, wordt alleen de virtuele machines met de servicenaam van die opgehaald.  Als **servicenaam** leeg is, klikt u vervolgens alle virtuele machines opgehaald.

### <a name="startstop-virtual-machines-and-send-output"></a>Virtuele machines starten en stoppen en uitvoer te sturen

    # Start each of the stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # The VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # The VM needs to be started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # The VM failed to start, so send notice
                Write-Output ($VM.InstanceName + " failed to start")
            }
            else
            {
                # The VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

De volgende regels stap tot en met elke virtuele machine.  Eerst is de **PowerState** van de virtuele machine gecontroleerd om te zien als deze is gestart of gestopt, afhankelijk van het runbook.  Als deze nog in de doel-staat, is klikt u vervolgens een bericht verstuurd naar uitvoer, en wordt de runbook beëindigd.  Als dat niet zo is, klikt u vervolgens **Begin-AzureVM** of **Stoppen-AzureVM** wordt gebruikt om te proberen te starten of stoppen de virtuele machine met het resultaat van de aanvraag kunt invullen die zijn opgeslagen naar een variabele.  Een bericht wordt vervolgens verzonden naar uitvoer opgeven of de aanvraag starten of stoppen is ingediend.


## <a name="next-steps"></a>Volgende stappen

- Zie meer informatie over het werken met onderliggende runbooks [onderliggende runbooks in Azure automatisering](automation-child-runbooks.md) 
- Meer informatie over Uitvoerberichten tijdens de uitvoering van runbook en logboekregistratie om te helpen oplossen, raadpleegt u [Runbook uitvoer en berichten in Azure automatisering](automation-runbook-output-and-messages.md)
