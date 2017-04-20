<properties 
    pageTitle="Tekstuele runbooks in Azure automatisering bewerken"
    description="Dit artikel bevat verschillende procedures voor het werken met PowerShell en de PowerShell-werkstroom runbooks in Azure automatisering met de tekstuele editor."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="stevenka"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="02/23/2016"
    ms.author="magoedte;bwren" />

# <a name="editing-textual-runbooks-in-azure-automation"></a>Tekstuele runbooks in Azure automatisering bewerken

De tekstuele editor in Azure automatisering kan worden gebruikt om [PowerShell runbooks](automation-runbook-types.md#powershell-runbooks) en [PowerShell werkstroom runbooks](automation-runbook-types.md#powershell-workflow-runbooks)te bewerken. Dit heeft de veelgebruikte functies van andere code editors zoals intellisense en kleurcodering met extra speciale functies om u te helpen bij het openen van resources beschikbaar is in de runbooks.  Dit artikel vindt gedetailleerde stappen voor het uitvoeren van andere functies met deze editor.

De tekstuele editor bevat een functie code voor activiteiten, activa en onderliggende runbooks invoegen in een runbook. In plaats van typen in de code uzelf, kunt u selecteren in een lijst van beschikbare resources en de juiste code in het runbook ingevoegd.

Elke runbook in Azure automatisering heeft twee versies, concept en Published. U de conceptversie van het runbook bewerken en vervolgens publiceren zodat deze kan worden uitgevoerd. De gepubliceerde versie kan niet worden bewerkt. Zie [publiceren van een runbook](automation-creating-importing-runbook.md#publishing-a-runbook) voor meer informatie.

Als u wilt werken met [Grafische Runbooks](automation-runbook-types.md#graphical-runbooks), raadpleegt u de [grafische beschikbaar is in Azure automatisering](automation-graphical-authoring-intro.md).

## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Een runbook in de portal van Azure bewerken

Gebruik de volgende procedure een runbook voor bewerken in de tekstuele editor openen.

1. Selecteer in de portal Azure uw automatisering-account.
2. Klik op de tegel **Runbooks** om de lijst van runbooks te openen.
3. Klik op de naam van het runbook die u wilt bewerken en klik vervolgens op de knop **bewerken** .
6. De vereiste bewerkingen uitvoeren
7. Klik op **Opslaan** wanneer de bewerkingen voltooid zijn.
8. Klik op **publiceren** als u wilt dat de nieuwste conceptversie van het runbook worden gepubliceerd.

### <a name="to-insert-a-cmdlet-into-a-runbook"></a>Een cmdlet invoegen in een runbook

2. Klik in het tekenpapier van de tekstuele editor, plaats de cursor waar u de cmdlet plaatsen.
3. Vouw **Cmdlets** in het besturingselement voor de bibliotheek. 
3. Vouw de module met de cmdlet die u wilt gebruiken.
4. Klik met de rechtermuisknop op de cmdlet als u wilt invoegen en selecteer **toevoegen aan het tekenpapier**.  Als de cmdlet meer dan één parameter instellen heeft, wordt klikt u vervolgens de standaardset toegevoegd.  U kunt ook de cmdlet om te selecteren van een reeks verschillende parameter uitvouwen.
4. De code voor de cmdlet wordt ingevoegd met de volledige lijst met parameters.
5. Geef de juiste waarde in plaats van het gegevenstype omgeven door accolades <> voor alle vereiste parameters.  Verwijder eventuele parameters die u niet nodig hebt.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Code voor een onderliggende runbook invoegen in een runbook

2. Klik in het tekenpapier van de tekstuele editor, plaats de cursor waar u de code voor het [onderliggende runbook](automation-child-runbooks.md)plaatsen.
3. Vouw het knooppunt **Runbooks** in het besturingselement voor de bibliotheek. 
3. Klik met de rechtermuisknop op het runbook als u wilt invoegen en selecteer **toevoegen aan het tekenpapier**.
4. De code voor het onderliggende runbook wordt ingevoegd met een tijdelijke aanduidingen voor parameters runbook.
5. De tijdelijke aanduidingen vervangen door een passende waarden voor elke parameter.

### <a name="to-insert-an-asset-into-a-runbook"></a>Activa invoegen in een runbook

2. Klik in het tekenpapier van de tekstuele editor, plaats de cursor waar u de code voor het onderliggende runbook plaatsen.
3. Vouw **activa** in het besturingselement voor de bibliotheek. 
4. Vouw voor het type van activum.
3. Klik met de rechtermuisknop op de activum als u wilt invoegen en selecteer **toevoegen aan het tekenpapier**.  Selecteer **toevoegen ' variabele krijgen "canvas** of **toevoegen ' variabele instellen" canvas** afhankelijk van wat u wilt ophalen of instellen van de variabele voor [variabele activa](automation-variables.md).
4. De code voor het activum wordt ingevoegd in het runbook.



## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Een runbook in de portal van Azure bewerken

Gebruik de volgende procedure een runbook voor bewerken in de tekstuele editor openen.

1. Klik in de portal Azure **automatisering** selecteren en vervolgens klik vervolgens op de naam van een automatisering-account.
2. Selecteer het tabblad **Runbooks** .
3. Klik op de naam van het runbook die u wilt bewerken en selecteer vervolgens het tabblad **auteur** .
5. Klik op de knop **bewerken** onder aan het scherm.
6. De vereiste bewerkingen uitvoeren
7. Klik op **Opslaan** wanneer de bewerkingen voltooid zijn.
8. Klik op **publiceren** als u wilt dat de nieuwste conceptversie van het runbook worden gepubliceerd.

### <a name="to-insert-an-activity-into-a-runbook"></a>Een activiteit in een Runbook invoegen

1. Klik in het tekenpapier van de tekstuele editor, plaats de cursor waar u de activiteit te plaatsen.
1. Klik op **Invoegen** en klik vervolgens op **activiteit**onderaan in het scherm.
1. Selecteer de module waarin de activiteit in de kolom **Integratiemodule** .
1. Klik in het deelvenster **activiteit** een activiteit te selecteren.
1. Let op de beschrijving van de activiteit in de kolom **Beschrijving** . (Optioneel) u kunt klikken op weergave gedetailleerde help voor het starten van de help voor de activiteit in de browser.
1. Klik op de pijl-rechts.  Als de activiteit parameters heeft, wordt deze voor uw gegevens worden weergegeven.
1. Klik op de knop controleren.  Code uitvoeren van de activiteit worden ingevoegd in het runbook.
1. Als de activiteit parameters vereist, bieden u een geschikte waarde in plaats van het gegevenstype omgeven door accolades <>.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Code voor een onderliggende runbook invoegen in een runbook

1. Klik in het tekenpapier van de tekstuele editor, plaats de cursor waar u het [onderliggende runbook](automation-child-runbooks.md)plaatsen.
2. Klik op **Invoegen** en klik vervolgens op **Runbook**onderaan in het scherm.
3. Selecteer het runbook van de middelste kolom invoegen en klik op de pijl-rechts.
4. Als het runbook parameters heeft, wordt deze voor uw gegevens worden weergegeven.
5. Klik op de knop controleren.  Code uitvoeren van het geselecteerde runbook worden ingevoegd in het huidige runbook.
7. Als het runbook parameters vereist, bieden u een geschikte waarde in plaats van het gegevenstype omgeven door accolades <>.

### <a name="to-insert-an-asset-into-a-runbook"></a>Activa invoegen in een runbook

1. Klik in het tekenpapier van de tekstuele editor, plaats de cursor waar u de activiteit om op te halen van de activa plaatsen.
1. Klik op **Invoegen** en klik vervolgens op **instelling**onderaan in het scherm.
1. Selecteer in de kolom **Action instelling** de actie die u wilt.
1. Selecteer in de beschikbare elementen in de middelste kolom.
1. Klik op de knop controleren.  Code ophalen of instellen van de activa worden ingevoegd in het runbook.



## <a name="to-edit-an-azure-automation-runbook-using-windows-powershell"></a>Bewerken van een Azure automatisering runbook via Windows PowerShell

Als u wilt bewerken een runbook met Windows PowerShell, gebruikt u de editor van uw keuze en sla deze op een .ps1-bestand. U kunt de cmdlet [Get-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/getazurerunbookdefinition) gebruiken om op te halen van de inhoud van het runbook en klik op het bestaande concept runbook vervangen door de gewijzigde-cmdlet [Set-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/setazurerunbookdefinition) .

### <a name="to-retrieve-the-contents-of-a-runbook-using-windows-powershell"></a>De inhoud van een Runbook via Windows PowerShell wilt ophalen

De volgende opdrachten in de steekproef laten zien hoe het script voor een runbook ophalen en opslaan als naar een scriptbestand. In dit voorbeeld zijn de conceptversie opgehaald. Het is ook mogelijk om op te halen van de gepubliceerde versie van het runbook Hoewel deze versie kan niet worden gewijzigd.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"
    
    $runbookDefinition = Get-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Slot Draft
    $runbookContent = $runbookDefinition.Content

    Out-File -InputObject $runbookContent -FilePath $scriptPath

### <a name="to-change-the-contents-of-a-runbook-using-windows-powershell"></a>De inhoud van een Runbook via Windows PowerShell wijzigen

De volgende opdrachten in de steekproef laten zien hoe de bestaande inhoud van een runbook vervangen door de inhoud van een scriptbestand. Houd er rekening mee dat dit de procedure voor de steekproef zoals in het [importeren van een runbook uit een scriptbestand met Windows PowerShell is](../automation-creating-or-importing-a-runbook#ImportRunbookScriptPS).

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    Set-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Path $scriptPath -Overwrite
    Publish-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName

## <a name="related-articles"></a>Verwante artikelen

- [Maken of importeren van een runbook in Azure automatisering](automation-creating-importing-runbook.md)
- [Werkstroom voor het onderwijs PowerShell](automation-powershell-workflow.md)
- [Grafische beschikbaar is in Azure automatisering](automation-graphical-authoring-intro.md)
- [Certificaten](automation-certificates.md)
- [Verbindingen](automation-connections.md)
- [Referenties](automation-credentials.md)
- [Schema 's](automation-schedules.md)
- [Variabelen](automation-variables.md)