<properties
    pageTitle="Maken of importeren van een runbook in Azure automatisering"
    description="In dit artikel wordt beschreven hoe u een nieuwe runbook in Azure automatisering wilt maken of importeren vanuit een bestand."
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
    ms.author="magoedte;bwren" />

# <a name="creating-or-importing-a-runbook-in-azure-automation"></a>Maken of importeren van een runbook in Azure automatisering

U kunt een runbook toevoegen aan Azure automatisering beide [maken van een nieuw](#creating-a-new-runbook) of als u een bestaande runbook importeert uit een bestand of vanuit de [Galerie met Runbook](automation-runbook-gallery.md). In dit artikel vindt u informatie over het maken en runbooks uit een bestand importeren.  U kunt alle details over de toegang tot community runbooks en modules in [Runbook en module galerieën voor Azure automatisering](automation-runbook-gallery.md)krijgen.

## <a name="creating-a-new-runbook"></a>Een nieuwe runbook maken

U kunt een nieuwe runbook maken in Azure automatisering met een van de Azure portals of Windows PowerShell. Zodra het runbook is gemaakt, kunt u deze informatie in het [Onderwijs PowerShell werkstroom](automation-powershell-workflow.md) en [grafische beschikbaar is in Azure automatisering](automation-graphical-authoring-intro.md)via kunt bewerken.

### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-classic-portal"></a>Een nieuwe Azure automatisering runbook maken met de klassieke Azure-portal

U kunt alleen met [PowerShell werkstroom runbooks](automation-runbook-types.md#powershell-workflow-runbooks) werken in de portal van Azure.

1. In de klassieke Azure-portal op klikt, **nieuwe** **App Services**, **automatisering**, **Runbook**, **Snelle maken**.
2. Voer de vereiste informatie en klik vervolgens op **maken**. De naam van de runbook moet beginnen met een letter en kan hebben letters, cijfers, onderstrepingstekens en streepjes.
3. Als u het runbook nu bewerken wilt, klikt u met een **Runbook bewerken**. Klik op **OK**.
4. Uw nieuwe runbook wordt weergegeven op het tabblad **Runbooks** .


### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-portal"></a>Een nieuwe Azure automatisering runbook maken met de portal van Azure

1. Klik in de portal Azure uw automatisering-account te openen.
2. Klik op de tegel **Runbooks** om de lijst van runbooks te openen.
3. Klik op de knop **toevoegen een runbook** en klik vervolgens op **maken een nieuwe runbook**.
2. Typ een **naam** voor het runbook en selecteert u het [Type](automation-runbook-types.md). De naam van de runbook moet beginnen met een letter en kan hebben letters, cijfers, onderstrepingstekens en streepjes.
3. Klik op **maken** voor het maken van het runbook en de editor openen.


### <a name="to-create-a-new-azure-automation-runbook-with-windows-powershell"></a>Een nieuwe Azure automatisering runbook maken met Windows PowerShell

U kunt de cmdlet [New-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt619376.aspx) een lege [PowerShell werkstroom runbook](automation-runbook-types.md#powershell-workflow-runbooks)maken. U kunt de parameter **Name** als u wilt maken van een lege runbook die u later kunt bewerken opgeven of kunt u de parameter **Path** als een runbook-bestand wilt importeren. De parameter **Type** moet ook worden opgenomen om op te geven op een van de vier runbook typen.

Het volgende voorbeeldopdrachten weergeven hoe u een nieuwe lege runbook maakt.

    New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
    -Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell

## <a name="importing-a-runbook-from-a-file-into-azure-automation"></a>Een runbook importeren uit een bestand in Azure automatisering

U kunt een nieuwe runbook in Azure automatisering maken door deze te importeren van een PowerShell-script of PowerShell werkstroom (.ps1 extensie) of een geëxporteerde grafische runbook (.graphrunbook).  U moet het [type runbook](automation-runbook-types.md) die worden gemaakt van het importeren rekening houdend met de volgende overwegingen opgeven.

- Een bestand .graphrunbook kan alleen worden geïmporteerd in een nieuwe [grafische runbook](automation-runbook-types.md#graphical-runbooks)en grafische runbooks kan alleen worden gemaakt vanuit een bestand .graphrunbook.
- Een .ps1-bestand met een werkstroom voor het PowerShell kan alleen worden geïmporteerd in een [werkstroom voor het PowerShell runbook](automation-runbook-types.md#powershell-workflow-runbooks).  Als het bestand meerdere PowerShell werkstromen bevat, klikt u vervolgens, mislukt het importeren. U moet elke werkstroom opslaan in een apart bestand en elk afzonderlijk importeren.
- Een .ps1-bestand dat een werkstroom niet bevat kan worden geïmporteerd in een [PowerShell-runbook](automation-runbook-types.md#powershell-runbooks) of een [PowerShell-werkstroom runbook](automation-runbook-types.md#powershell-workflow-runbooks).  Als deze worden geïmporteerd in een werkstroom voor het PowerShell-runbook, klikt u vervolgens deze worden geconverteerd naar een werkstroom en opmerkingen worden opgenomen in het runbook opgeven van de wijzigingen die zijn aangebracht.

### <a name="to-import-a-runbook-from-a-file-with-the-azure-classic-portal"></a>Een runbook importeren uit een bestand met de klassieke Azure-portal
De volgende procedure kunt u een scriptbestand importeren in Azure automatisering.  Houd er rekening mee dat u alleen een .ps1-bestand kunt importeren in een werkstroom voor het PowerShell-runbook met behulp van deze portal.  U moet de Azure-portal gebruiken voor andere typen.

1. Klik in de portal Azure Management **automatisering** selecteren en selecteer een Account automatisering.
2. Klik op **importeren**.
3. Klik op **Zoeken naar een bestand** en zoek het scriptbestand te importeren.
4. Als u het runbook nu bewerken wilt, klikt u met een **Runbook bewerken**. Klik op OK.
5. Het nieuwe runbook wordt weergegeven op het tabblad **Runbooks** voor het Account dat automatisering.
6. U moet [het runbook publiceren](#publishing-a-runbook) voordat u deze kunt uitvoeren.


### <a name="to-import-a-runbook-from-a-file-with-the-azure-portal"></a>Een runbook importeren uit een bestand met de portal van Azure
De volgende procedure kunt u een scriptbestand importeren in Azure automatisering.  

>[AZURE.NOTE] Houd er rekening mee dat u alleen een .ps1-bestand kunt importeren in een werkstroom voor het PowerShell-runbook met behulp van de portal.

1. Klik in de portal Azure uw automatisering-account te openen.
2. Klik op de tegel **Runbooks** om de lijst van runbooks te openen.
3. Klik op de knop **toevoegen een runbook** en klik vervolgens op **importeren**.
4. Klik op **Runbook bestand** om het bestand te importeren te selecteren
2. Als het veld **naam** is ingeschakeld, hebt u de optie om deze te wijzigen.  De naam van de runbook moet beginnen met een letter en kan hebben letters, cijfers, onderstrepingstekens en streepjes.
3. Het [type runbook](automation-runbook-types.md) automatisch wordt geselecteerd, maar u kunt het type wijzigen na het maken van de toepassing beperkingen in aanmerking. 
3. Het nieuwe runbook wordt weergegeven in de lijst met runbooks voor het Account dat automatisering.
4. U moet [het runbook publiceren](#publishing-a-runbook) voordat u deze kunt uitvoeren.

>[AZURE.NOTE] Nadat u een grafische runbook of een grafische PowerShell werkstroom runbook importeert, hebt u de optie converteren naar het andere type als het gewenste type. U kunt geen converteren naar tekst.

### <a name="to-import-a-runbook-from-a-script-file-with-windows-powershell"></a>Een runbook importeren uit een script-bestand met Windows PowerShell

Een scriptbestand importeren als concept PowerShell werkstroom runbook kunt u de cmdlet [Importeren-AzureRMAutomationRunbook](https://msdn.microsoft.com/library/mt603735.aspx) . Als het runbook al bestaat, mislukt het importeren tenzij u de *-dwingen* parameter. 

De volgende opdrachten in de steekproef laten zien hoe een scriptbestand importeren in een runbook.

    $automationAccountName =  "AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
    $RGName = "ResourceGroup"

    Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
    -ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
    -Type PowerShellWorkflow 


## <a name="publishing-a-runbook"></a>Een runbook publiceren

Wanneer u maken of importeren van een nieuwe runbook, moet u deze publiceren voordat u deze kunt uitvoeren.  Elke runbook in automatisering heeft een voorlopige versie en een gepubliceerde versie. Alleen de gepubliceerde versie is kunnen worden uitgevoerd en alleen de conceptversie kan worden bewerkt. De gepubliceerde versie wordt niet beïnvloed door wijzigingen in de conceptversie. Als de conceptversie moet beschikbaar worden gemaakt, klikt u vervolgens publiceert u deze die de versie Published met de conceptversie overschrijft.

## <a name="to-publish-a-runbook-using-the-azure-classic-portal"></a>Een runbook met behulp van de Azure klassieke portal publiceren

1. Open het runbook in de klassieke Azure-portal.
1. Aan de bovenkant van het scherm, klikt u op **auteur**.
1. Klik onderaan het scherm op **publiceren** en klik vervolgens **Ja** aan het bericht.

## <a name="to-publish-a-runbook-using-the-azure-portal"></a>Een runbook met behulp van de Azure portal publiceren

1. Open het runbook in de portal van Azure.
1. Klik op de knop **bewerken** .
1. Klik op de knop **publiceren** en vervolgens **Ja** aan het bericht.


## <a name="to-publish-a-runbook-using-windows-powershell"></a>Een runbook via Windows PowerShell publiceren

U kunt de cmdlet [Publiceren AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603705.aspx) publiceren van een runbook met Windows PowerShell. De volgende opdrachten in de steekproef hoe publiceren van een steekproef runbook.

    $automationAccountName =  AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $RGName = "ResourceGroup"

    Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
    -Name $runbookName -ResourceGroupName $RGName


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over hoe u van het Runbook en de galerie met PowerShell-Module profiteren kunt, raadpleegt u [Runbook en module galerieën voor het automatiseren van Azure](automation-runbook-gallery.md)
- Zie voor meer informatie over het bewerken van PowerShell en de PowerShell-werkstroom runbooks met een tekstuele editor, [tekstuele runbooks bewerken in Azure automatisering](automation-edit-textual-runbook.md)
- Zie meer informatie over grafische runbook authoring, [grafische beschikbaar is in Azure automatisering](automation-graphical-authoring-intro.md)
