<properties 
   pageTitle="Planningen in Azure automatisering | Microsoft Azure"
   description="Automatisering planningen worden gebruikt voor het plannen van runbooks in Azure automatisering automatisch wordt gestart. Beschrijving van het maken en beheren van een planning in, zodat u een runbook automatisch op een bepaald moment of op een terugkerende planning starten kunt."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="mgoedtel" />

# <a name="scheduling-a-runbook-in-azure-automation"></a>Een runbook in Azure automatisering plannen

Voor het plannen van een runbook in Azure automatisering laten beginnen met een bepaalde tijd koppelen u deze aan een of meer schema's. Een planning in op uitvoeren op een opnieuw optreedt of één keer per uur of dagelijkse planning voor runbooks in de portal van Azure klassieke en voor runbooks in de portal van Azure kan worden geconfigureerd, kunt u bovendien plannen ze voor wekelijks, maandelijks, specifieke dagen van de week of dagen van de maand of een bepaalde dag van de maand.  Een runbook kan worden gekoppeld aan meerdere planningen en een planning meerdere runbooks die zijn gekoppeld aan dit kan hebben.

>[AZURE.NOTE]  Schema's ondersteunen momenteel niet Azure automatisering DSC configuraties.

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell-Cmdlets

De cmdlets in de volgende tabel worden gebruikt om te maken en beheren van schema's met Windows PowerShell in Azure automatisering. Wordt verzonden als onderdeel van de [Azure PowerShell-module](../powershell-install-configure.md).

|Cmdlets voor|Beschrijving|
|:---|:---|
|**Azure resourcemanager-cmdlets**||
|[Get-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603733.aspx)|Een planning zijn opgehaald.|
|[Nieuwe AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx)|Hiermee maakt u een nieuwe planning.|
|[Verwijderen AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603691.aspx)|Hiermee verwijdert u een planning.|
|[Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx)|Hiermee stelt u de eigenschappen voor een bestaande planning.|
|[Get-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt619406.aspx)|Haalt gepland runbooks.|
|[Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx)|Een runbook koppelt aan een planning.|
|[Unregister AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603844.aspx)|Een runbook van een planning dissociates.|
|**Azure servicebeheer-cmdlets**||
|[Get-AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690274.aspx)|Een planning zijn opgehaald.|
|[Nieuwe AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690271.aspx)|Hiermee maakt u een nieuwe planning.|
|[Verwijderen AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690279.aspx)|Hiermee verwijdert u een planning.|
|[Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690270.aspx)|Hiermee stelt u de eigenschappen voor een bestaande planning.|
|[Get-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn913778.aspx)|Haalt gepland runbooks.|
|[Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn690265.aspx)|Een runbook koppelt aan een planning.|
|[Unregister AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn690273.aspx)|Een runbook van een planning dissociates.|

## <a name="creating-a-schedule"></a>Een planning maken

U kunt een nieuwe planning voor runbooks maken in de portal van Azure, in de klassieke portal of met Windows PowerShell. U hebt ook de optie voor het maken van een nieuwe planning wanneer u een runbook koppelt aan een planning met behulp van de Azure klassieke of Azure-portal.

>[AZURE.NOTE] Wanneer u een schema aan een runbook koppelen, wordt er in automatisering slaat de huidige versies van de modules in uw account en deze koppelingen naar die planning.  Dit betekent dat als u een module met versie 1.0 had in uw account wanneer u een schema gemaakt en vervolgens de module naar versie 2.0 bijwerken, de planning blijft 1.0 gebruiken.  Pas de bijgewerkte moduleversie gebruiken, moet u een nieuwe planning maken. 

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Een nieuwe planning maken in de portal van Azure

1. Klik in de portal Azure van uw account automatisering, klikt u op de tegel **activa** om te openen van het blad **activa** .
2. Klik op de tegel **planningen** om te openen van het blad **planningen** .
3. Klik op **een planning toevoegen** aan de bovenkant van het blad.
4. Typ een **naam** en eventueel ook een **Beschrijving** voor de nieuwe planning op het blad **nieuwe planning** .
5. Selecteren of de planning één keer wordt uitgevoerd, of op een terugkerende planning door **eenmaal** of **Terugkeerpatroon**te selecteren.  Als u **één keer** selecteert, Geef een **Begintijd** en klik vervolgens op **maken**.  Als u **Terugkeerpatroon**selecteert, Geef een **Begintijd** en de frequentie voor hoe vaak u wilt dat het runbook bovenaan elke pagina: per **uur**, **dag**, **week**of per **maand**.  Als u **week** of **maand** uit de vervolgkeuzelijst selecteert, het **Terugkeerpatroon optie** wordt weergegeven in het blad en bij het selecteren, het **Terugkeerpatroon optie** blad krijgt en u kunt de dag van week selecteren als u **week**hebt geselecteerd.  Als u **maand**hebt geselecteerd, kunt u kiezen op de **dag van de week** of specifieke dagen van de maand in de agenda en ten slotte u wilt uitvoeren op de laatste dag van de maand of niet en klik vervolgens op **OK**.   

### <a name="to-create-a-new-schedule-in-the-azure-classic-portal"></a>Een nieuwe planning maken in de klassieke Azure-portal

1. In de portal voor Azure klassieke automatisering selecteren en selecteer vervolgens de naam van een automatisering-account.
1. Selecteer het tabblad **activa** .
1. Klik onder aan het venster, op **Instelling toevoegen**.
1. Klik op **planning toevoegen**.
1. Typ een **naam** en desgewenst een **Beschrijving** voor de nieuwe schedule.your planning **Één keer**, **per uur**, **dagelijks**, **Wekelijks**of **maandelijks**wordt uitgevoerd.
1. Geef een **Begintijd** en andere opties, afhankelijk van het type planning die u hebt geselecteerd.

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Een nieuwe planning maken met Windows PowerShell

U kunt de cmdlet [New-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) maken van een nieuwe planning in Azure automatisering voor klassieke runbooks of [Nieuw AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) -cmdlet gebruiken voor runbooks in de portal van Azure. Moet u de starttijd van de planning en de frequentie die deze moet worden uitgevoerd.

De volgende opdrachten in de steekproef ziet hoe u een schema voor het 15e en 30e van elke maand met een resourcemanager Azure-cmdlet maken.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"

De volgende opdrachten in de steekproef weergeven het maken van een nieuwe planning die wordt uitgevoerd van elke dag bij 3:30 PM op 20 januari 2015 beginnen met een cmdlet Azure servicebeheer.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

## <a name="linking-a-schedule-to-a-runbook"></a>Een planning koppelen aan een runbook

Een runbook kan worden gekoppeld aan meerdere planningen en een planning meerdere runbooks die zijn gekoppeld aan dit kan hebben. Als een runbook parameters heeft, kunt u de waarden voor deze opgeven. U moet waarden opgeven voor parameters die verplicht en waarden voor elke optionele parameters kan bepalen.  Deze waarden worden gebruikt telkens wanneer die het runbook door deze planning wordt gestart.  U kunt het dezelfde runbook als bijlage toevoegen aan een ander schema en andere parameterwaarden opgeven.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Een planning koppelen aan een runbook in de portal van Azure

1. Klik in de portal Azure van uw account automatisering, klikt u op de tegel **Runbooks** om te openen van het blad **Runbooks** .
2. Klik op de naam van het runbook om te plannen.
3. Als het runbook is momenteel niet gekoppeld aan een planning, klikt u vervolgens krijgt u de optie voor het maken van een nieuwe planning of een koppeling naar een bestaande planning.  
4. Als het runbook parameters heeft, kunt u de optie **Modify instellingen (standaard: Azure) worden uitgevoerd** en het blad **Parameters** wordt gepresenteerd waar kunt u de gegevens dienovereenkomstig gewijzigd.  

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-portal"></a>Een planning koppelen aan een runbook met Azure klassieke portal

1. In de portal voor Azure klassieke **automatisering** selecteren en klik vervolgens klikt u vervolgens op de naam van een account automatisering.
2. Selecteer het tabblad **Runbooks** .
3. Klik op de naam van het runbook om te plannen.
4. Klik op het tabblad **planning** .
5. Als het runbook is momenteel niet gekoppeld aan een planning, klikt u vervolgens krijgt u de optie naar de **koppeling naar een nieuwe planning** of een **koppeling naar een bestaande planning**.  Als het runbook momenteel is gekoppeld aan een planning, klikt u op **koppeling** onderaan in het venster voor toegang tot deze opties.
6. Als het runbook parameters heeft, wordt u gevraagd hun waarden.  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Een planning koppelen aan een runbook met Windows PowerShell

U kunt de [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) een schema koppelen aan een klassieke runbook of de [Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) -cmdlet voor het runbooks in de portal van Azure.  U kunt waarden voor de parameters van het runbook opgeven met de parameter Parameters. Zie [een Runbook in Azure automatisering starten](automation-starting-a-runbook.md) voor meer informatie over het parameterwaarden opgeven.

De volgende opdrachten in de steekproef laten zien hoe een schema koppelen aan een runbook met een resourcemanager Azure-cmdlet en parameters.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"
Het volgende voorbeeldopdrachten weergeven het koppelen van een planning met een servicebeheer Azure-cmdlet en parameters.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

## <a name="disabling-a-schedule"></a>Een planning uitschakelen

Wanneer u een planning uitschakelt, kan eventuele runbooks die is gekoppeld aan dit niet meer op schema worden uitgevoerd. U kunt handmatig een planning uitschakelen of instellen van een verlooptijd voor schema's met een frequentie wanneer u deze hebt gemaakt. Wanneer de verlooptijd is bereikt, wordt de planning uitgeschakeld.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Voor het uitschakelen van een planning van de Azure-portal

1. Klik in de portal Azure van uw account automatisering, klikt u op de tegel **activa** om te openen van het blad **activa** .
2. Klik op de tegel **planningen** om te openen van het blad **planningen** .
2. Klik op de naam van een schema voor het blad details openen.
3. Wijzig de **ingeschakeld** op **Nee**.

### <a name="to-disable-a-schedule-from-the-azure-classic-portal"></a>Een planning van de Azure klassieke portal uitschakelen

U kunt een planning in de klassieke Azure-portal op de pagina planning voor de planning uitschakelen.

1. In de portal voor Azure klassieke automatisering selecteren en klik vervolgens klikt u vervolgens op de naam van een account automatisering.
1. Selecteer het tabblad activa.
1. Klik op de naam van een planning om de detailpagina te openen.
2. Wijzig de **ingeschakeld** op **Nee**.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>Een planning met Windows PowerShell uitschakelen

De cmdlet [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) kunt u de eigenschappen van een bestaand schema voor een klassiek runbook of de cmdlet [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) voor runbooks in de portal van Azure wijzigen. Als u wilt uitschakelen in de planning, geef **Onwaar** voor de parameter **IsEnabled** .

De volgende opdrachten in de steekproef laten zien hoe een schema voor een runbook met een resourcemanager Azure-cmdlet uitschakelen.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"

De volgende opdrachten in de steekproef laten zien hoe een planning met de cmdlet beheer van Azure-Service uitschakelen.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

## <a name="next-steps"></a>Volgende stappen

- Als u wilt beginnen met runbooks in Azure automatisering, Zie [een Runbook in Azure automatisering starten](automation-starting-a-runbook.md) 