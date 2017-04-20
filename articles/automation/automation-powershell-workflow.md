<properties 
   pageTitle="Werkstroom voor het onderwijs PowerShell"
   description="In dit artikel is bedoeld als een snelle les voor auteurs vertrouwd met PowerShell voor meer informatie over de specifieke verschillen tussen PowerShell en de PowerShell-werkstroom."
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
   ms.date="09/12/2016"
   ms.author="bwren" />

# <a name="learning-windows-powershell-workflow"></a>Werkstroom voor het onderwijs Windows PowerShell

Runbooks in Azure automatisering worden geïmplementeerd als Windows PowerShell werkstromen.  Een Windows PowerShell-werkstroom is vergelijkbaar met een Windows PowerShell-script maar enkele belangrijke verschillen die naar een nieuwe gebruiker kunnen verwarrend heeft.  In dit artikel is bedoeld voor gebruikers bekend zijn met PowerShell en kort wordt uitgelegd dat u nodig hebt als u een PowerShell-script naar een PowerShell-werkstroom voor gebruik in een runbook converteren wilt concepten.  

Een werkstroom is een reeks geprogrammeerde, verbonden stappen die langdurige taken uitvoeren of de afhankelijk van meerdere stappen over meerdere apparaten of beheerde knooppunten vereisen. De voordelen van een werkstroom via een normale script omvatten de mogelijkheid een actie ten opzichte van meerdere apparaten tegelijk wordt uitgevoerd en de mogelijkheid automatisch herstellen uit de fouten. Een Windows PowerShell-werkstroom is een Windows PowerShell-script die worden gebruikt in Windows Workflow Foundation. Terwijl de werkstroom is geschreven met de syntaxis van de Windows PowerShell en dat is gestart door Windows PowerShell, wordt deze verwerkt door Windows Workflow Foundation.

Zie [Aan de slag met Windows PowerShell Workflow](http://technet.microsoft.com/library/jj134242.aspx)voor uitgebreide informatie over de onderwerpen in dit artikel.

## <a name="types-of-runbook"></a>Soorten runbook

Er zijn drie soorten runbook in Azure automatisering, *PowerShell werkstroom*, *PowerShell* en *grafische*.  U kunt het type runbook definiëren wanneer u het runbook maken en u kunt een runbook niet naar het andere type converteren zodra deze is gemaakt.

PowerShell werkstroom runbooks en PowerShell runbooks voor gebruikers die werken liever rechtstreeks met de PowerShell-code ofwel gebruikt de tekstuele editor in Azure automatisering of een offline-editor, zoals PowerShell ISE. Als u een werkstroom voor het PowerShell-runbook maakt, moet u de informatie in dit artikel begrijpen. 

Grafische runbooks kunt u een runbook dezelfde activiteiten en cmdlets maar werkt met een grafische gebruikersinterface dat de complexiteit van de onderliggende PowerShell-werkstroom verbergt maken.  Grafische runbooks concepten in dit artikel zoals controlepunten en parallelle uitvoering nog steeds toepassen, maar hoeft u niet over de syntaxis van de gedetailleerde zorgen te maken. 

## <a name="basic-structure-of-a-workflow"></a>Basisstructuur van een werkstroom

De eerste stap bij het converteren van een PowerShell-script naar een PowerShell-werkstroom is tussen met het trefwoord **werkstroom** .  Een werkstroom wordt gestart met de **werkstroom voor het** trefwoord gevolgd door de hoofdtekst van het script tussen accolades. De naam van de werkstroom volgt het trefwoord **werkstroom** , zoals wordt weergegeven in de volgende syntaxis. 

    Workflow Test-Workflow
    {
       <Commands>
    }

De naam van de werkstroom moet overeenkomen met de naam van het runbook automatisering. Als het runbook worden geïmporteerd, klikt u vervolgens de bestandsnaam moet overeenkomen met de naam van de werkstroom en moet eindigen op .ps1.

Als u wilt parameters toevoegen aan de werkstroom, door het sleutelwoord **parameter** te gebruiken, net zoals u zou naar een script doen. 

## <a name="code-changes"></a>Codewijzigingen

PowerShell werkstroom code Hiermee wordt gezocht vrijwel hetzelfde als de PowerShell-scriptcode, behalve voor een paar ingrijpend wijzigt.  De volgende secties worden de wijzigingen die u moet aanbrengen in een PowerShell-script voor deze om uit te voeren in een werkstroom.

### <a name="activities"></a>Activiteiten

Een activiteit is een specifieke taak in een werkstroom. Net zoals een script uit een of meer opdrachten bestaat, een werkstroom bestaat uit een of meer activiteiten die worden uitgevoerd in een reeks weergeven. Windows PowerShell werkstroom omgezet automatisch in veel van de Windows PowerShell-cmdlets activiteiten tijdens het uitvoeren van een werkstroom. Wanneer u een van deze cmdlets in uw runbook opgeeft, wordt de bijbehorende activiteit werkelijk uitvoeren door Windows Workflow Foundation. Voor deze cmdlets zonder een bijbehorende activiteit, Windows PowerShell werkstroom automatisch wordt uitgevoerd de cmdlet binnen een activiteit [InlineScript](#inlinescript) . Er is een verzameling cmdlets die niet worden opgenomen en kunnen niet worden gebruikt in een werkstroom, tenzij u ze expliciet in een blok InlineScript opnemen. Zie voor meer informatie over deze concepten, [Activiteiten in Script werkstromen gebruiken](http://technet.microsoft.com/library/jj574194.aspx).

Werkstroomactiviteiten delen een set met algemene parameters voor het configureren van de werking. Zie voor meer informatie over de werkstroom algemene parameters, [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).

### <a name="positional-parameters"></a>Positionele parameters

U kunt positionele parameters niet gebruiken met activiteiten en de bijbehorende cmdlets in een werkstroom.  Dit betekent dat is dat u parameternamen moet gebruiken.

Bekijk bijvoorbeeld de volgende code waarvoor u alle actieve services.

     Get-Service | Where-Object {$_.Status -eq "Running"}

Als u deze code in een werkstroom uitvoert wilt, krijgt u een bericht zoals "parameterset kan niet worden omgezet met de opgegeven parameters benoemde."  U kunt deze fout door de naam van de parameter zoals in de volgende gewoon te bieden.

    Workflow Get-RunningServices
    {
        Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
    }

### <a name="deserialized-objects"></a>Gedeserialiseerde objecten

Objecten in werkstromen zijn gedeserialiseerd.  Dit betekent dat hun eigenschappen zijn nog steeds beschikbaar, maar niet de methoden.  Bekijk bijvoorbeeld de volgende PowerShell-code die een service met behulp van de methode beëindiging van de Service-object stopt.

    $Service = Get-Service -Name MyService
    $Service.Stop()

Als u probeert om uit te voeren dit in een werkstroom, krijgt u een foutmelding 'oproepen methode wordt niet ondersteund in een Windows PowerShell-werkstroom' verschijnt.  

Eén optie is om te laten teruglopen van deze twee regels met code in een blok [InlineScript](#InlineScript) in dat geval $Service zou een serviceobject binnen de blokkering. 

    Workflow Stop-Service
    {
        InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
        }
    } 

Een andere optie is via een andere cmdlet waarmee u dezelfde functionaliteit als de methode, indien beschikbaar.  De cmdlet Stop-Service biedt dezelfde functionaliteit als de methode Stop voor ons voorbeeld, en u kunt de volgende manieren te werk voor een werkstroom.

    Workflow Stop-MyService
    {
        $Service = Get-Service -Name MyService
        Stop-Service -Name $Service.Name
    }


## <a name="inlinescript"></a>InlineScript

De activiteit **InlineScript** is handig wanneer u moet een of meer opdrachten uitvoeren als traditionele PowerShell-script in plaats van de PowerShell-werkstroom.  Terwijl de opdrachten in een werkstroom voor verwerking naar Windows Workflow Foundation worden verzonden, worden door Windows PowerShell-opdrachten in een blok InlineScript verwerkt. 

InlineScript worden de syntaxis van de onderstaande gebruikt.

    InlineScript
    {
      <Script Block>
    } <Common Parameters>

U kunt uitvoer van een InlineScript teruggaan door de uitvoer toewijzen aan een variabele. Het volgende voorbeeld een service stopt en vervolgens Hiermee kunt u de servicenaam van de.

    Workflow Stop-MyService
    {
        $Output = InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


U kunt waarden in een blok InlineScript doorgeven, maar u moet **$Using** bereik wijzigingstoets gebruiken.  Het volgende voorbeeld is gelijk aan het vorige voorbeeld, behalve dat de servicenaam van de wordt geleverd door een variabele. 

    Workflow Stop-MyService
    {
        $ServiceName = "MyService"
    
        $Output = InlineScript {
            $Service = Get-Service -Name $Using:ServiceName
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Hoewel InlineScript activiteiten mogelijk kritieke in bepaalde werkstromen, worden ze ondersteunen geen werkstroom constructies en mag alleen worden gebruikt wanneer dat nodig is voor de volgende oorzaken hebben:

- U kunt [controlepunten](#Checkpoints) binnen een blok InlineScript niet gebruiken. Als een fout binnen de blokkering optreedt, moet u deze hervat vanaf het begin van de blokkering.
- U kunt [parallelle uitvoering](#parallel-execution) binnen een InlineScriptBlock niet gebruiken.
- Schaalbaarheid van de werkstroom heeft invloed op InlineScript aangezien de Windows PowerShell-sessie voor de hele duur van de blokkering InlineScript bevat.

Zie [Windows PowerShell-opdrachten uitvoeren in een werkstroom](http://technet.microsoft.com/library/jj574197.aspx) en [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx)voor meer informatie over het gebruik van InlineScript.


## <a name="parallel-processing"></a>Parallelle verwerking

Een voordeel van Windows PowerShell werkstromen is een reeks opdrachten in plaats van parallel opeenvolging uitvoeren als u met een typisch script. 

U kunt het **parallelle** trefwoord maken van een scriptblok met meerdere opdrachten die tegelijkertijd worden uitgevoerd. Hiermee worden de syntaxis van de onderstaande gebruikt. In dit geval wordt Activity1 en Activity2 op hetzelfde moment starten. Activity3 begint pas nadat Activity1 zowel Activity2 hebt voltooid.

    Parallel
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>


Bekijk bijvoorbeeld de volgende PowerShell-opdrachten die meerdere bestanden naar de netwerkbestemming van een kopiëren.  Deze opdrachten zijn opeenvolging uitgevoerd zodat die één bestand kopiëren eindigen moet voordat het volgende wordt gestart.     

    $Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
    $Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
    $Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt

De volgende werkstroom wordt uitgevoerd van deze opdrachten parallel zodat deze alle beginnen op hetzelfde moment kopiëren.  Alleen nadat ze alle zijn wordt volledig hebt gekopieerd het voltooiingsbericht weergegeven.

    Workflow Copy-Files
    {
        Parallel 
        {
            $Copy-Item -Path "C:\LocalPath\File1.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File2.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File3.txt" -Destination "\\NetworkPath"
        }

        Write-Output "Files copied."
    }


U kunt de **ForEach-parallelle** constructie proces opdrachten voor elk item in een verzameling gelijktijdig. De items in de siteverzameling worden van parallel tijdens het uitvoeren van de opdrachten in het scriptblok opeenvolging. Hiermee worden de syntaxis van de onderstaande gebruikt. In dit geval wordt Activity1 gestart op hetzelfde moment voor alle items in de siteverzameling. Voor elk item begint Activity2 nadat Activity1 voltooid is. Activity3 begint pas nadat Activity1 zowel Activity2 voor alle items hebt voltooid.

    ForEach -Parallel ($<item> in $<collection>)
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>

Het volgende voorbeeld is vergelijkbaar met het vorige voorbeeld bestanden parallel kopiëren.  In dit geval wordt een bericht weergegeven voor elk bestand nadat deze zijn gekopieerd.  Alleen nadat ze alle zijn wordt volledig hebt gekopieerd het uiteindelijke voltooiingsbericht weergegeven.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach -Parallel ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
        }
        
        Write-Output "All files copied."
    }

> [AZURE.NOTE]  We raden niet onderliggende runbooks parallel uitgevoerd, aangezien dit blijkt dat het geven van onbetrouwbare resultaten.  De uitvoer van het kind runbook soms niet wordt weergegeven en instellingen in één onderliggende runbook invloed kunnen zijn op de andere parallelle onderliggende runbooks 


## <a name="checkpoints"></a>Controlepunten

Een *Samenvatting* is een momentopname van de huidige status van de werkstroom met de huidige waarde voor variabelen en uitvoer gegenereerd aan dat punt. Als u een werkstroom voor het eindigt op fout of is geschorst, wordt klikt u vervolgens de volgende keer dat deze wordt uitgevoerd gestart vanaf de laatste controlepunt in plaats van het begin van de worfklow.  U kunt een controlepunt instellen in een werkstroom met de activiteit **Controlepunt-werkstroom** .

In de volgende code, een uitzondering voor cijfergroepen na Activity2 veroorzaakt door de werkstroom beëindigen. Wanneer de werkstroom opnieuw wordt uitgevoerd, wordt deze gestart door Activity2 uit te voeren omdat dit vlak achter het laatste controlepunt instellen.

    <Activity1>
    Checkpoint-Workflow
    <Activity2>
    <Exception>
    <Activity3>

Nadat de activiteiten die mogelijk vatbaar voor uitzondering en niet moeten worden herhaald als de werkstroom wordt hervat, moet u de controlepunten set in een werkstroom. Uw werkstroom kan bijvoorbeeld een virtuele machine maken. U kunt een controlepunt kunt instellen, zowel voor en na de opdrachten voor het maken van de virtuele machine. Als de aanmaakdatum mislukt, zou klikt u vervolgens de opdrachten worden herhaald als de werkstroom opnieuw is gestart. Als de de worfklow nadat de aanmaakdatum is geslaagd, mislukt, en vervolgens de virtuele machine niet opnieuw gemaakt wordt wanneer de werkstroom wordt hervat.

Het volgende voorbeeld wordt gekopieerd van meerdere bestanden naar een netwerklocatie en stelt een controlepunt na elk bestand.  Als de netwerklocatie verbroken is, moet worden klikt u vervolgens de werkstroom beëindigd fout.  Wanneer u deze opnieuw wordt gestart, wordt deze weer op de laatste controlepunt wat betekent dat alleen de bestanden die al zijn gekopieerd wordt overgeslagen.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
            Checkpoint-Workflow
        }
        
        Write-Output "All files copied."
    }

Omdat gebruikersnaam referenties niet behouden zijn wanneer u de activiteit [Onderbreking-werkstroom](https://technet.microsoft.com/library/jj733586.aspx) hebt gebeld, of na de laatste controlepunt, moet u de referenties op null en klik vervolgens op te halen ze opnieuw uit het archief activa nadat **Onderbreking-werkstroom** of controlepunt wordt genoemd.  Anders verschijnt het volgende foutbericht weergegeven: *de werkstroomtaak kan niet worden voortgezet, omdat permanente gegevens kan niet worden opgeslagen volledig of opgeslagen permanente gegevens heeft is beschadigd. Moet u de werkstroom opnieuw opstarten.*

De volgende dezelfde code ziet hoe u omgaat met dit in uw runbooks PowerShell-werkstroom.

       
    workflow CreateTestVms
    {
       $Cred = Get-AzureAutomationCredential -Name "MyCredential"
       $null = Add-AzureRmAccount -Credential $Cred

       $VmsToCreate = Get-AzureAutomationVariable -Name "VmsToCreate"

       foreach ($VmName in $VmsToCreate)
         {
          # Do work first to create the VM (code not shown)
        
          # Now add the VM
          New-AzureRmVm -VM $Vm -Location "WestUs" -ResourceGroupName "ResourceGroup01"

          # Checkpoint so that VM creation is not repeated if workflow suspends
          $Cred = $null
          Checkpoint-Workflow
          $Cred = Get-AzureAutomationCredential -Name "MyCredential"
          $null = Add-AzureRmAccount -Credential $Cred
         }
     } 


Dit is niet vereist als u zich aanmelden met een geconfigureerd met een service principal account in het vak uitvoeren als.  

Zie voor meer informatie over controlepunten, [Controlepunten toevoegen aan een werkstroom voor het Script](http://technet.microsoft.com/library/jj574114.aspx).


## <a name="next-steps"></a>Volgende stappen

- Als u wilt beginnen met PowerShell werkstroom runbooks, raadpleegt u [Mijn eerste runbook voor PowerShell-werkstroom](automation-first-runbook-textual.md) 
