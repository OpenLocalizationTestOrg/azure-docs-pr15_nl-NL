<properties 
   pageTitle="Onderliggende runbooks in Azure automatisering | Microsoft Azure"
   description="Beschrijving van de verschillende methoden voor het starten van een runbook in Azure automatisering uit een andere runbook en delen van informatie ertussen."
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
   ms.date="08/17/2016"
   ms.author="magoedte;bwren" />

# <a name="child-runbooks-in-azure-automation"></a>Onderliggende runbooks in Azure automatisering

Het is een goede gewoonte in Azure automatisering herbruikbare, modulaire runbooks met een afzonderlijke functie die kan worden gebruikt door andere runbooks schrijven. Een bovenliggende runbook belt vaak een of meer onderliggende runbooks om uit te voeren vereiste functionaliteit. Er zijn twee manieren om te bellen van een onderliggende runbook en elk onderling nogal verschillen die u moet kennen heeft zodat u kunt bepalen welke beste werkt voor uw verschillende scenario's.

##  <a name="invoking-a-child-runbook-using-inline-execution"></a>Aanroepen van een onderliggende runbook met inline worden uitgevoerd

Om aan te roepen een inline runbook uit een andere runbook, gebruikt u de naam van het runbook en verstrek waarden voor de parameters precies zoals u een activiteit of de cmdlet gebruiken zou.  Alle runbooks in hetzelfde account automatisering zijn beschikbaar voor alle andere moet worden gebruikt op deze manier. Het runbook bovenliggende wacht tot het onderliggende runbook om te voltooien voordat u verdergaat naar de volgende regel en eventuele uitvoergegevens rechtstreeks naar de bovenliggende.

Als u een inline runbook aanroept, wordt deze uitgevoerd in dezelfde taak als de bovenliggende runbook. Er worden geen vermelding in de geschiedenis van het kind runbook die deze hebt uitgevoerd. Uitzonderingen de eventuele uitzonderingen en eventuele stream uitvoer van het runbook onderliggende zijn gekoppeld aan de bovenliggende. Dit levert minder taken en maakt ze gemakkelijker om bij te houden en op te lossen omdat de eventuele uitzonderingen door het onderliggende runbook en een van de stream-uitvoer zijn gekoppeld aan de bovenliggende taak.

Wanneer een runbook wordt gepubliceerd, moet elke onderliggende runbooks dat wordt aangeroepen al worden gepubliceerd. Dit komt doordat Azure Automatisering een koppeling met een onderliggend runbooks genereert wanneer een runbook is gecompileerd. Als ze geen, wordt het runbook bovenliggende publiceren juist wordt weergegeven, maar genereert een uitzondering wanneer deze wordt gestart. Als dit gebeurt, kunt u het runbook bovenliggende opnieuw publiceren om te kunnen de onderliggende runbooks correct verwijzen. U hoeft niet te publiceren van het runbook bovenliggende als een van de onderliggende runbooks worden gewijzigd omdat de koppeling wordt al zijn gemaakt.

De parameters van een onderliggende runbook genoemd inline kunnen een gegevenstype inclusief complexe objecten zijn, en er geen [JSON serialisatie](automation-starting-a-runbook.md#runbook-parameters) omdat er tijdens het starten van het runbook via Azure Management Portal of met de cmdlet Start-AzureRmAutomationRunbook.


### <a name="runbook-types"></a>Runbook typen

Welke typen kunnen bellen elkaar:

- Een [PowerShell-runbook](automation-runbook-types.md#powershell-runbooks) en [grafische runbooks](automation-runbook-types.md#graphical-runbooks) kunt bellen elke andere inline (beide zijn gebaseerd PowerShell).
- Een [werkstroom voor het PowerShell runbook](automation-runbook-types.md#powershell-workflow-runbooks) en de grafische PowerShell-werkstroom runbooks kunt elke andere inline (beide zijn PowerShell werkstroom die is gebaseerd) bellen
- De PowerShell-typen en de typen PowerShell elkaar inline kunnen niet worden aangeroepen en begin-AzureRmAutomationRunbook moeten gebruiken.
    
Als volgorde belangrijk wordt gepubliceerd:

- De volgorde van het publiceren van runbooks is alleen van belang voor PowerShell-werkstroom en de grafische PowerShell werkstroom runbooks.


Wanneer u een grafische of PowerShell werkstroom onderliggende runbook inline worden uitgevoerd met belt, gebruikt u alleen de naam van het runbook.  Wanneer u een PowerShell onderliggende runbook belt, moet u de naam ervan met voorafgegaan *.\\ * om op te geven dat het script bevindt zich in de lokale adreslijst. 

### <a name="example"></a>Voorbeeld

Het volgende voorbeeld wordt een test onderliggende runbook die drie parameters, een complexe objecten, een geheel getal en een Booleaanse waarde accepteert. De uitvoer van het runbook onderliggende wordt toegewezen aan een variabele.  In dit geval is het onderliggende runbook een runbook PowerShell-werkstroom

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true

Hier volgt een PowerShell-runbook gebruiken als de onderliggende hetzelfde voorbeeld.

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = .\PS-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true



##  <a name="starting-a-child-runbook-using-cmdlet"></a>Een onderliggend runbook met cmdlet starten

U kunt de [Begin-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) -cmdlet starten een runbook zoals is beschreven in [een runbook met Windows PowerShell starten](../automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell). Er zijn twee manieren gebruiken voor deze cmdlet.  De cmdlet retourneert de taak-id in de ene, zodra de onderliggende taak is gemaakt voor het onderliggende runbook.  In de andere modus, waarmee u door het opgeven van de **-Wacht** parameter, de cmdlet wordt wachten tot de onderliggende taak is voltooid en de uitvoer van het kind runbook zullen retourneren.

De taak van een onderliggende runbook de slag met een cmdlet wordt uitgevoerd in een afzonderlijk taak van het runbook bovenliggende. Dit levert meer taken dan het aanroepen van het runbook inline en wordt omgezet in moeilijker om bij te houden. De bovenliggende kunt meerdere onderliggende runbooks asynchroon starten zonder te wachten op elk om te voltooien. Voor die hetzelfde soort parallelle uitvoering bellen van de onderliggende runbooks inline, moet het runbook bovenliggende gebruik het [trefwoord parallelle](automation-powershell-workflow.md#parallel-processing).

Parameters voor een onderliggende runbook de slag met een cmdlet zijn die is opgegeven als een hashtable zoals is beschreven in [Runbook Parameters](automation-starting-a-runbook.md#runbook-parameters). Alleen eenvoudige gegevenstypen kunnen worden gebruikt. Als het runbook een parameter met een complexe gegevenstype heeft, moet klikt u vervolgens deze worden aangeroepen inline.

### <a name="example"></a>Voorbeeld

Het volgende voorbeeld een onderliggende runbook begint met parameters en wacht deze met behulp van de begin-AzureRmAutomationRunbook-parameter wacht. Als voltooid, worden de uitvoer van het runbook onderliggende verzameld.

    $params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true} 
    $joboutput = Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-ChildRunbook" -ResouceGroupName "LabRG" –Parameters $params –wait


## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Vergelijking van methoden voor het bellen van een onderliggende runbook

De volgende tabel bevat een overzicht van de verschillen tussen de twee methoden voor het bellen van een runbook uit een andere runbook.

| | Inline| Cmdlet|
|:---|:---|:---|
|Taak|Onderliggende runbooks worden uitgevoerd in dezelfde taak als de bovenliggende.|Een afzonderlijk taak is gemaakt voor het onderliggende runbook.|
|Kan worden uitgevoerd|Bovenliggende runbook wacht voor het onderliggende runbook om te voltooien voordat u verdergaat.|Bovenliggende runbook blijft direct nadat onderliggende runbook wordt gestart *of* bovenliggende runbook wacht in de onderliggende taak is voltooid.|
|Uitvoer|Bovenliggende runbook kunt rechtstreeks krijgen voor uitvoer van onderliggende runbook.|Bovenliggende runbook uitvoer moet ophalen uit de onderliggende runbook taak *of* bovenliggende runbook kunt rechtstreeks krijgen uitvoer van onderliggende runbook.|
|Parameters|Waarden voor de onderliggende runbook parameters zijn opgegeven afzonderlijk en een gegevenstype kunnen gebruiken.|Waarden voor de onderliggende runbook parameters moeten worden gecombineerd in één hashtable en alleen als eenvoudige opnemen kunnen, matrix en object-gegevenstypen die middelen JSON serialisatie.|
|Automatisering-Account|Bovenliggende runbook kunt onderliggende runbook alleen gebruiken in hetzelfde automatisering-account.|Bovenliggende runbook kunt onderliggende runbook van elk account automatisering uit hetzelfde Azure-abonnement en zelfs een ander abonnement als ik heb een verbinding met deze.|
|Publiceren|Onderliggende runbook moet worden gepubliceerd voordat bovenliggende runbook wordt gepubliceerd.|Onderliggende runbook moet worden gepubliceerd als elk gewenst moment voordat bovenliggende runbook wordt gestart.|

## <a name="next-steps"></a>Volgende stappen

- [Een runbook starten in Azure automatisering](automation-starting-a-runbook.md)
- [Runbook uitvoer en berichten in Azure automatisering](automation-runbook-output-and-messages.md)
