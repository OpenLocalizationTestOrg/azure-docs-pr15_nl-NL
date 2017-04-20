<properties 
   pageTitle="Runbook-instellingen"
   description="Beschrijving van de configuratie-instellingen voor een runbook in Azure automatisering en hoe u deze gebruik van de Azure Management Portal en de Windows PowerShell wijzigt."
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

# <a name="runbook-settings"></a>Runbook-instellingen

Elke runbook in Azure automatisering heeft meerdere instellingen waarmee deze kan worden geïdentificeerd en het gedrag van logboekregistratie wijzigen. Elk van deze instellingen wordt hieronder beschreven gevolgd door de procedures voor het wijzigen van deze.

## <a name="settings"></a>Instellingen

### <a name="name-and-description"></a>Naam en beschrijving

U kunt de naam van een runbook niet meer wijzigen nadat deze is gemaakt. De beschrijving kan is optioneel en maximaal 512 tekens.

### <a name="tags"></a>Labels

Labels kunnen u afzonderlijke woorden en zinnen om vast te stellen van een runbook toewijzen. Bijvoorbeeld wanneer u een runbook aan de [Galerie met Runbook](https://msdn.microsoft.com/library/dn781422.aspx)indient, u bepaalde tags om te bepalen welke categorieën het runbook moet worden weergegeven in. U kunt meerdere tags voor een runbook opgeven door deze te scheiden met komma's.

### <a name="logging"></a>Logboekregistratie

Standaard worden uitgebreid en de voortgang van records niet naar werkervaring geschreven. U kunt de instellingen voor een bepaalde runbook wilt Meld u aan deze records wijzigen. Zie voor meer informatie over deze records, [Runbook uitvoer en berichten](https://msdn.microsoft.com/library/dn879148.aspx).

## <a name="changing-runbook-settings"></a>Runbook-instellingen wijzigen

### <a name="changing-runbook-settings-with-the-azure-management-portal"></a>Runbook wijzigen met de beheerportal van Azure

U kunt instellingen voor een runbook in de beheerportal Azure wijzigen van de pagina **configureren** voor het runbook.

1. In de beheerportal Azure **automatisering** selecteren en de naam van een account automatisering klik vervolgens op.
1. Selecteer het tabblad **Runbooks** .
1. Klik op de naam van een runbook.
1. Selecteer het tabblad **configureren** .

### <a name="changing-runbook-settings-with-windows-powershell"></a>Runbook wijzigen met Windows PowerShell

De cmdlet [Set-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690275.aspx) kunt u de instellingen wijzigen voor een runbook. Als u opgeven van meerdere tags wilt, kunt u een matrix of één tekenreeks met door komma's gescheiden waarden aan de parameter Tags verstrekken. U kunt de huidige tags met de [Get-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690278.aspx)krijgen.

De volgende opdrachten in de steekproef laten zien hoe de eigenschappen voor een runbook instellen. In dit voorbeeld wordt drie tags toegevoegd aan de bestaande tags en geeft aan dat uitgebreide records moeten worden vastgelegd.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="related-articles"></a>Verwante artikelen
- [Runbook uitvoer en berichten](../automation-runbook-output-and-messages) 
- [Maken of importeren van een Runbook](https://msdn.microsoft.com/library/dn643637.aspx) 