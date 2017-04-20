<properties
 pageTitle="Knooppunten berekeningscluster HPC Pack beheren | Microsoft Azure"
 description="Meer informatie over de hulpmiddelen voor de PowerShell-script toevoegen, verwijderen, starten en stoppen HPC Pack berekeningscluster knooppunten in Azure wordt aangegeven"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="manage-the-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Het aantal en de beschikbaarheid van berekeningscluster knooppunten in een cluster HPC Pack in Azure beheren

Als u een HPC Pack cluster in Azure VMs hebt gemaakt, wilt u misschien manieren eenvoudig toevoegen, verwijderen, (gegevens leveren) starten of stoppen (identiteitsgegevens) een aantal berekeningscluster knooppunt VMs in het cluster. Deze taken doet Azure PowerShell-scripts die zijn geïnstalleerd op het hoofd knooppunt VM worden uitgevoerd. Deze scripts kunnen u het aantal en de beschikbaarheid van resources HPC Pack cluster bepalen, zodat u kosten kunt bepalen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## <a name="prerequisites"></a>Vereisten voor

* **HPC Pack cluster in Azure VMs** - een HPC Pack cluster in het implementatiemodel klassieke maken met behulp van ten minste HPC Pack 2012 R2 Update 1. U kunt bijvoorbeeld de implementatie automatiseren met behulp van de huidige HPC Pack VM afbeelding in de Azure Marketplace en een Azure PowerShell-script. Zie voor informatie en vereisten voor [een HPC Cluster met het implementatiescript HPC Pack IaaS maken](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

    Na implementatie, zoek het knooppunt management scripts in de map % CCP\_start % bin-map op de kop knooppunt. U kunt elk van de scripts moet uitvoeren als beheerder.

* **Azure publiceren instellingen bestand of management certificaat** - moet u een van de volgende handelingen uit op het hoofd knooppunt handelingen uit:

    * **Importeren de Azure instellingenbestand publiceren**. Hiervoor voert u de volgende Azure PowerShell-cmdlets op het hoofd knooppunt:

    ```
    Get-AzurePublishSettingsFile

    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```

    * **Het beheer van Azure-certificaat op het hoofd knooppunt configureren**. Als u de .cer-bestand hebt, importeren in het archief CurrentUser\My en voer de volgende Azure PowerShell-cmdlet voor uw Azure-omgeving (AzureCloud of AzureChinaCloud):

    ```
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a>Berekeningscluster knooppunt VMs toevoegen

Berekeningscluster knooppunten met het script **Toevoegen-HpcIaaSNode.ps1** toevoegen.

### <a name="syntax"></a>Syntaxis
```
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a>Parameters

* **Servicenaam** - naam van de cloud service dat nieuwe berekeningscluster knooppunt VMs, worden toegevoegd aan.

* **ImageName** - Azure VM afbeelding naam, die kan worden verkregen via de portal van Azure klassieke of Azure PowerShell-cmdlet **Get-AzureVMImage**. De afbeelding moet voldoen aan de volgende vereisten:

    1. Een Windows-besturingssysteem moet zijn geïnstalleerd.

    2. HPC Pack moet zijn geïnstalleerd in de rol van de knooppunt berekeningscluster.

    3. De afbeelding moet een persoonlijke afbeelding in de categorie van de gebruiker, niet in een openbare Azure VM afbeelding.

* **Aantal** - aantal berekeningscluster knooppunt VMs moet worden opgeteld.

* **InstanceSize** - grootte van het knooppunt berekeningscluster VMs.

* **DomainUserName** - gebruiker domeinnaam, die wordt gebruikt voor de nieuwe VMs toevoegen aan het domein.

* **DomainUserPassword** - wachtwoord van de domeingebruiker.

* **NodeNameSeries** (optioneel) - patroon naamgevingsconventies voor de knooppunten berekeningscluster. De opmaak moet worden &lt; *hoofdsite\_naam*&gt;&lt;*starten\_getal*&gt;%. Bijvoorbeeld: MyCN % 10% betekent dat een reeks met de namen van berekeningscluster knooppunt vanaf MyCN11. Als niet opgeeft, wordt het script gebruikt de geconfigureerde knooppunt naming reeks in het cluster HPC.

### <a name="example"></a>Voorbeeld

Het volgende voorbeeld wordt 20 grootte grote berekeningscluster knooppunt VMs in de cloud service *hpcservice1*, op basis van de VM afbeelding *hpccnimage1*.

```
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a>Berekeningscluster knooppunt VMs verwijderen

Verwijder berekeningscluster knooppunten met het script **Verwijderen-HpcIaaSNode.ps1** .

### <a name="syntax"></a>Syntaxis

```
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a>Parameters

* **Naam** - namen van knooppunten zijn verwijderd. Jokertekens worden ondersteund. De naam van de parameter is de naam. U kunt geen zowel de **naam** en het **knooppunt** parameters opgeven.

* **Knooppunt** - object het HpcNode voor de knooppunten moet worden verwijderd, die kan worden verkregen tot en met de HPC PowerShell-cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). De naam van de parameter is knooppunt. U kunt geen zowel de **naam** en het **knooppunt** parameters opgeven.

* **DeleteVHD** (optioneel) - instellen voor het verwijderen van de bijbehorende schijven voor de VMs die zijn verwijderd.

* **Dwingen** (optioneel) - instellen voor het afdwingen van HPC knooppunten offline voordat u deze verwijderen.

* **Bevestigen** (optioneel) - vragen om bevestiging voordat het uitvoeren van de opdracht.

* **WhatIf** - instellen om te beschrijven wat gebeurt er als u de opdracht uitgevoerd zonder de opdracht daadwerkelijk wordt uitgevoerd.

### <a name="example"></a>Voorbeeld

Het volgende voorbeeld forceert u offline knooppunten met namen die beginnen *HPCNode-CN -* en deze verwijderd van de knooppunten en hun bijbehorende schijven.

```
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a>Start berekeningscluster knooppunt VMs

Begin berekeningscluster knooppunten met het script **Start-HpcIaaSNode.ps1** .

### <a name="syntax"></a>Syntaxis

```
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a>Parameters

* **Naam** - namen van de knooppunten om te worden gestart. Jokertekens worden ondersteund. De naam van de parameter is de naam. U kunt geen zowel de **naam** en het **knooppunt** parameters opgeven.

* **Knooppunt**- object het HpcNode voor de knooppunten om te worden gestart, die kan worden verkregen tot en met de HPC PowerShell-cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). De naam van de parameter is knooppunt. U kunt geen zowel de **naam** en het **knooppunt** parameters opgeven.

### <a name="example"></a>Voorbeeld

Het volgende voorbeeld begint knooppunten met namen die beginnen *HPCNode-CN -*.

```
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a>Berekeningscluster knooppunt VMs stoppen

Stop berekenen knooppunten met het script **Stoppen-HpcIaaSNode.ps1** .

### <a name="syntax"></a>Syntaxis

```
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a>Parameters


* **Naam**- namen van knooppunten moet worden gestopt. Jokertekens worden ondersteund. De naam van de parameter is de naam. U kunt geen zowel de **naam** en het **knooppunt** parameters opgeven.

* **Knooppunt** - object het HpcNode voor de knooppunten moet worden gestopt die kan worden verkregen tot en met de HPC PowerShell-cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). De naam van de parameter is knooppunt. U kunt geen zowel de **naam** en het **knooppunt** parameters opgeven.

* **Dwingen** (optioneel) - instellen voor het afdwingen van HPC knooppunten offline voordat deze wordt gestopt.

### <a name="example"></a>Voorbeeld

Het volgende voorbeeld forceert u offline knooppunten met namen die beginnen *HPCNode-CN -* en klikt u vervolgens de knooppunten stopt.

Stop-HPCIaaSNode.ps1 – naam HPCNodeCN-*-dwingen

## <a name="next-steps"></a>Volgende stappen

* Als u wilt dat een manier automatisch groter of kleiner knooppunten op basis van de huidige belasting van projecten en taken op het cluster, raadpleegt u [automatisch groter en kleiner maken van de bronnen HPC Pack cluster in Azure op basis van de werklast cluster](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).
