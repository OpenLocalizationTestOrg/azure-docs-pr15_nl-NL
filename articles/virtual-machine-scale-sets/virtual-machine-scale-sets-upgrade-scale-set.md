<properties
    pageTitle="Een app op VM schaal sets implementeren | Microsoft Azure"
    description="Een app op VM schaal sets implementeren"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="guybo"/>


# <a name="upgrade-a-virtual-machine-scale-set"></a>Upgraden van een virtuele machine schaal instellen

In dit artikel wordt beschreven hoe u kunt implementeren van een update OS een schaalfactor van Azure virtuele machines zonder eventuele uitvaltijd instellen. In deze context heeft betrekking op een update OS wijzigen van de versie of de SKU van het besturingssysteem of het wijzigen van de URI van een aangepaste afbeelding. Bijwerken zonder downtime virtuele machines één tegelijk of in groepen (zoals een domein foutenstructuuranalyse per keer) in plaats van in één keer bijwerken. Als u dit doet, kan een virtuele machines die niet worden bijgewerkt houden uitgevoerd.

Als u wilt voorkomen dat dubbelzinnigheden, laten we onderscheiden drie soorten OS bijwerken die u wilt uitvoeren:

- De versie of de SKU van een afbeelding platform te wijzigen. Bijvoorbeeld wijzigen Ubuntu 14.04.2-LTS versie van 14.04.201506100 14.04.201507060, of de Ubuntu 15.10/laatste SKU te 16.04.0-LTS/latest wijzigen. Dit scenario wordt beschreven in dit artikel.

- Wijzigen van de URI die naar een nieuwe versie van een aangepaste afbeelding u verwijst ingebouwd (**Eigenschappen** > **virtualMachineProfile** > **storageProfile** > **osDisk** > **afbeelding** > **uri**). Dit scenario wordt beschreven in dit artikel.

- Het besturingssysteem van binnen een virtuele machine herstellen (voorbeelden hiervan zijn een beveiligingspatch installeren en uitvoeren van Windows Update). Dit scenario wordt ondersteund, maar niet wordt beschreven in dit artikel.

De eerste twee opties zijn ondersteunde vereisten bestrijkt in dit artikel. U moet maken van een nieuwe schaal instellen voor het uitvoeren van de derde optie.

VM schaal sets die zijn geïmplementeerd als onderdeel van een cluster [Azure Service stof](https://azure.microsoft.com/services/service-fabric/) vallen niet hier.

De volgorde van de eenvoudige voor het wijzigen van de OS-versie/SKU van de afbeelding van een platform de URI van een aangepaste afbeelding ziet er als volgt uit:

1. Krijg het model VM schaal instellen.

2. Wijzig de versie, SKU of URI waarde in het model.

3. Werk het model.

4. Een gesprek *manualUpgrade* in de virtuele machines in de set schaal doen. Deze stap is alleen relevant als *upgradePolicy* is ingesteld op **handmatig** in een set schaal. Als deze is ingesteld op **automatisch**, is het zijn dat de virtuele machines tegelijk bijgewerkt hetgeen downtime.


Met deze achtergrondinformatie in gedachten, laten we eens kijken hoe u de versie van een schaal in PowerShell en met behulp van de REST API instellen kan bijwerken. In deze voorbeelden begeleidende de hoofdletters/kleine letters van de afbeelding van een platform, maar in dit artikel vindt u kunt dit proces voor een aangepaste afbeelding aanpassen.

## <a name="powershell"></a>PowerShell##

In dit voorbeeld wordt een Windows-VM schaal instellen naar de nieuwe versie 4.0.20160229 bijgewerkt. Nadat het model zijn bijgewerkt, wordt een update één VM exemplaar tegelijk.

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get the VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update the virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

Als u de URI voor een aangepaste afbeelding in plaats van het wijzigen van een versie van de afbeelding platform bijwerken wilt, moet u de regel 'zet de nieuwe versie' vervangen door ongeveer als volgt te werk:

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```


## <a name="the-rest-api"></a>De REST API

Hier volgen een paar Python voorbeelden die de Azure REST API gebruiken om te implementeren een update van de versie OS. Beide gebruiken de bibliotheek lightweight [azurerm](https://pypi.python.org/pypi/azurerm) van Azure REST API Wikkel functies een ophalen op het model schaal instellen, gevolgd door een opslag met een bijgewerkte model. Ze ook bekijken VM exemplaren weergaven aan te geven van de virtuele machines door update domein.

### <a name="vmssupgrade"></a>Vmssupgrade

 [Vmssupgrade](https://github.com/gbowerman/vmsstools) is een script Python die wordt gebruikt voor het implementeren van een upgrade OS een actieve VM schaalfactor één update domain tegelijkertijd ingesteld.

![Vmssupgrade script voor het kiezen van virtuele machines of een domein bijwerken](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

Dit script kunt u kiezen specifieke virtuele machines om te werken of een update-domein opgeven. Biedt ondersteuning voor een versie van de afbeelding platform wijzigen of het wijzigen van de URI van een aangepaste afbeelding.

### <a name="vmsseditor"></a>Vmsseditor

[Vmsseditor](https://github.com/gbowerman/vmssdashboard) is een algemene editor voor VM schaal sets met geeft van VM status als een heatmap, waarbij één rij één update domain vertegenwoordigt. U kunt onder andere het model voor een set schaal bijwerken met een nieuwe versie, SKU of aangepaste afbeelding URI en kiest u foutenstructuuranalyse domeinen upgrade uit te voeren. Als u dit doet, wordt de virtuele machines in dat domein update worden bijgewerkt naar het nieuwe model. U kunt ook een lopende upgrade op basis van de batchgrootte van uw keuze doen.  

De volgende schermafbeelding ziet u een model van een schaal instellen voor Ubuntu 14.04-2LTS versie 14.04.201507060. Veel meer opties zijn toegevoegd aan dit hulpmiddel sinds deze schermafbeelding is geplaatst.

![Vmsseditor model van een schaal instellen voor Ubuntu 14.04-2LTS](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

Nadat u op **upgraden** en klik vervolgens op **Details ophalen**, gaat virtuele machines in per 0 bijwerken.

![Vmsseditor waarin update wordt uitgevoerd](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)
