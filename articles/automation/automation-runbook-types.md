<properties 
   pageTitle="Azure automatisering Runbook typen"
   description="Beschrijft de verschillende typen runbooks die u in Azure automatisering en overwegingen waarmee u rekening houden moet bij het bepalen van welk type u kunt gebruiken. "
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

# <a name="azure-automation-runbook-types"></a>Azure automatisering runbook typen

Azure-automatisering ondersteunt vier typen runbooks die worden kort beschreven in de volgende tabel.  De onderstaande secties bevatten meer informatie over elke inclusief overwegingen op welk type.


| Type |  Beschrijving |
|:---|:---|
| [Grafische](#graphical-runbooks) | Op basis van Windows PowerShell en gemaakt en bewerkt volledig in grafische editor in Azure-portal. | 
| [Grafische PowerShell-werkstroom](#graphical-runbooks) | Op basis van Windows PowerShell-werkstroom en gemaakt en bewerkt volledig in de grafische editor in Azure-portal. 
| [PowerShell](#powershell-runbooks) | Tekst runbook op basis van Windows PowerShell-script.
| [PowerShell-werkstroom](#powershell-workflow-runbooks) | Tekst runbook op basis van Windows PowerShell-werkstroom. |


## <a name="graphical-runbooks"></a>Grafische runbooks

[Grafische](automation-runbook-types.md#graphical-runbooks) en grafische PowerShell werkstroom runbooks zijn gemaakt en bewerkt met de grafische editor in de portal van Azure.  U kunt deze naar een bestand exporteren en vervolgens importeren in een ander automatisering-account, maar u kunt maken of bewerken met een ander hulpmiddel.  Grafische runbooks PowerShell-code genereren, maar u kunt geen rechtstreeks weergeven of wijzigen van de code. Grafische runbooks kan niet worden geconverteerd naar een van de [tekstopmaak](automation-runbook-types.md), noch kan een runbook tekst worden omgezet in een grafische indeling. Grafische runbooks kunnen worden geconverteerd naar grafische PowerShell werkstroom runbooks tijdens het importeren en vice versa.

### <a name="advantages"></a>Voordelen

- Maak runbooks met minimale kennis van [PowerShell](automation-powershell-workflow.md).
- Processen visueel voor te stellen.
- Gebruiken in andere runbooks onderliggende runbooks om hoog niveau werkstromen te maken.


### <a name="limitations"></a>Beperkingen

- Kan geen runbook buiten Azure-portal niet bewerken.
- Kan vragen om de activiteit in een Code met PowerShell-code als u wilt uitvoeren van complexe logica.
- Kan bekijken of de PowerShell-code die is gemaakt door de grafische werkstroom rechtstreeks bewerken. Houd er rekening mee dat u de code die u in een Code-activiteiten maakt kunt weergeven.


## <a name="powershell-runbooks"></a>PowerShell runbooks

PowerShell runbooks zijn gebaseerd op de Windows PowerShell.  U bewerken de code van het runbook met de teksteditor in de portal van Azure rechtstreeks.  U kunt ook een offline teksteditor en het [importeren van het runbook](http://msdn.microsoft.com/library/azure/dn643637.aspx) in Azure automatisering gebruiken.

### <a name="advantages"></a>Voordelen

- Implementeer alle complexe logica met PowerShell-code zonder de extra complexiteit van PowerShell-werkstroom. 
- Runbook begint sneller dan grafische of PowerShell werkstroom runbooks omdat deze geen hoeft te worden gecompileerd afgespeeld.

### <a name="limitations"></a>Beperkingen

- Moet vertrouwd zijn met PowerShell-scripts.
- Niet [parallelle verwerking](automation-powershell-workflow.md#parallel-processing) gebruiken om uit te voeren meerdere acties parallel.
- Niet [controlepunten](automation-powershell-workflow.md#checkpoints) niet runbook voor het geval de fout weer wilt gebruiken.
- PowerShell werkstroom runbooks en grafische runbooks kunnen alleen worden opgenomen als onderliggende runbooks met behulp van de begin-AzureAutomationRunbook-cmdlet waarmee een nieuwe taak wordt.

### <a name="known-issues"></a>Bekende problemen
Huidige bekende problemen met PowerShell runbooks Hier volgen.

- PowerShell runbooks kan niet een niet-versleutelde [variabele activa](automation-variables.md) met een null-waarde niet kunt ophalen.
- Een [variabele activa](automation-variables.md) met kan niet worden opgehaald door PowerShell runbooks *~* in de naam.
- Get-proces doorlopend wordt afgespeeld in een PowerShell runbook loopt vast na ongeveer 80 iteraties. 
- Een PowerShell-runbook mislukt mogelijk als u probeert een zeer grote hoeveelheid gegevens in één keer naar de stream uitvoer schrijven.   U kunt meestal dit probleem omzeilen door het uitvoeren van alleen de gegevens die u nodig hebt tijdens het werken met grote objecten.  In plaats van het uitvoeren van ongeveer zo *Get-proces*, kunt u bijvoorbeeld alleen de vereiste velden met *Get-proces uitvoeren | Selecteer procesnaam, CPU*.

## <a name="powershell-workflow-runbooks"></a>PowerShell werkstroom runbooks

PowerShell werkstroom runbooks zijn tekst runbooks op basis van [Windows PowerShell-werkstroom](automation-powershell-workflow.md).  U bewerken de code van het runbook met de teksteditor in de portal van Azure rechtstreeks.  U kunt ook een offline teksteditor en het [importeren van het runbook](http://msdn.microsoft.com/library/azure/dn643637.aspx) in Azure automatisering gebruiken.

### <a name="advantages"></a>Voordelen

- Alle complexe logica met PowerShell werkstroom code implementeren.
- [Controlepunten](automation-powershell-workflow.md#checkpoints) runbook voor het geval de fout weer wilt gebruiken.
- [Parallelle verwerking](automation-powershell-workflow.md#parallel-processing) gebruiken om meerdere acties parallel.
- Andere grafische runbooks en PowerShell werkstroom runbooks kunt opnemen als onderliggende runbooks om hoog niveau werkstromen te maken.


### <a name="limitations"></a>Beperkingen

- Auteur moet vertrouwd zijn met PowerShell-werkstroom.
- Runbook moet hebben betrekking op de extra complexiteit van een werkstroom voor het PowerShell zoals [gedeserialiseerd objecten](automation-powershell-workflow.md#code-changes).
- Runbook duurt langer om te beginnen dan PowerShell runbooks omdat deze nodig hebt voordat u opgesteld.
- PowerShell runbooks kunnen alleen worden opgenomen onderliggende runbooks met behulp van de begin-AzureAutomationRunbook-cmdlet waarmee een nieuwe taak wordt.


## <a name="considerations"></a>Overwegingen

U moet rekening de volgende aanvullende overwegingen bij het bepalen van welk type u voor een bepaalde runbook.

- U converteren niet runbooks van grafische naar tekst typen of vice versa.
- Er zijn beperkingen runbooks van verschillende typen als een onderliggende runbook gebruiken.  Zie [onderliggende runbooks in Azure automatisering](automation-child-runbooks.md) voor meer informatie.

  
## <a name="next-steps"></a>Volgende stappen

- Zie meer informatie over grafische runbook authoring, [grafische beschikbaar is in Azure automatisering](automation-graphical-authoring-intro.md)
- Voor meer informatie over de verschillen tussen PowerShell en PowerShell werkstromen voor runbooks, Zie [Leren Windows PowerShell-werkstroom](automation-powershell-workflow.md)
- Zie voor meer informatie over het maken of importeren van een Runbook, [maken of importeren van een Runbook](automation-creating-importing-runbook.md)



