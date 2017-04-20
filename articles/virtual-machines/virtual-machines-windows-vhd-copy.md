<properties
    pageTitle="Een kopie maken van een gespecialiseerde VM in Azure | Microsoft Azure"
    description="Leer hoe u een kopie van een gespecialiseerde Windows VM uitvoeren in Azure, in het implementatiemodel resourcemanager maken."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>
    
    
    
# <a name="create-a-copy-of-a-specialized-windows-vm-running-in-azure"></a>Een kopie van een gespecialiseerde Windows VM uitgevoerd in Azure maken 

In dit artikel leest u hoe u een kopie van de VHD maken vanuit een gespecialiseerde Windows VM die wordt uitgevoerd in Azure wordt aangegeven met het hulpmiddel AZCopy. Vervolgens kunt u in de kopie van de VHD maken van een nieuwe VM. 

- Als een generalized VM kopiëren, raadpleegt u [het maken van een afbeelding VM uit een bestaande generalized Azure VM](virtual-machines-windows-capture-image.md)wilt.

- Als u een VHD vanuit een on-premises implementatie VM, zoals een gemaakt met behulp van Hyper-V, het [uploaden van een Windows-VHD vanuit een on-premises implementatie VM naar Azure](virtual-machines-windows-upload-image.md)Zie uploaden wilt.


## <a name="before-you-begin"></a>Voordat u begint

Zorg ervoor dat u:

- Informatie over de **bron- en doeltabellen opslag-accounts**hebt. Voor de bron VM moet u de namen van de account en de container van de opslag. De containernaam is meestal **VHD's**. Ook moet u beschikken over een bestemming opslag-account. Als u dit niet al hebt, kunt u er met behulp van een van beide de portal maken (**Meer Services** > opslag accounts > toevoegen) of met de cmdlet [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) . 

- Azure [PowerShell 1.0](../powershell-install-configure.md) is (of hoger) is geïnstalleerd.

- Hebt gedownload en geïnstalleerd van het [hulpmiddel AzCopy](../storage/storage-use-azcopy.md). 


## <a name="deallocate-the-vm"></a>De VM toewijzing

De VM, waardoor de VHD moet worden gekopieerd systeembronnen toewijzing. 

- **Portal**: klik op **virtuele machines** > **myVM** > stoppen
- **PowerShell**: `Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM` deallocates de VM **myVM** in resource groep **myResourceGroup**met de naam.

De **Status** voor het VM in de portal van Azure gewijzigd van **gestopt** op **gestopt (opgeheven)**.


## <a name="get-the-storage-account-urls"></a>De URL van de account opslag ophalen

Moet u de URL's van de bron- en doeltabellen opslag-accounts. De URL's eruitzien: `https://<storageaccount>.blob.core.windows.net/<containerName>/`. Als u de naam van de opslag-account en container al weet, kunt u alleen de gegevens tussen de haakjes om te maken van de URL te vervangen. 

U kunt de portal van Azure of Azure Powershell gebruiken om de URL:

- **Portal**: klik op **meer services** > **opslag accounts**  >  <storage account> **BLOB's** en uw bron VHD-bestand is waarschijnlijk in de container **VHD's** . Klik op **Eigenschappen** voor de container en kopieer de **URL van**het label tekst. U moet de URL's van het bron- en doeltabellen containers. 

- **PowerShell**: `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` wordt de informatie voor VM **myVM** in de resource groep **myResourceGroup benoemde**. In de zoekresultaten, zoekt u in de sectie **opslag profiel** de **Vhd Uri**. Het eerste deel van de Uri is de URL voor de container en het laatste onderdeel is de naam van de OS VHD voor VM.

## <a name="get-the-storage-access-keys"></a>De toegangstoetsen opslag ophalen

Zoek de toegangstoetsen voor de bron- en doeltabellen opslag-accounts. Zie voor meer informatie over toegangstoetsen [over Azure opslag-accounts](../storage/storage-create-storage-account.md).

- **Portal**: klik op **meer services** > **opslag accounts**  >  <storage account> **Alle instellingen** > **toegangstoetsen**. Kopieer de toets met het label als **sleutel1**.
- **PowerShell**: `Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup` ontvangt van de opslag-toets voor de opslag account **mystorageaccount** in de resource groep **myResourceGroup**. Kopieer de toets met het label als **sleutel1**.


## <a name="copy-the-vhd"></a>Kopieer de VHD 

U kunt bestanden tussen opslag accounts met AzCopy kopiëren. Voor de bestemmingscontainer, als de opgegeven container niet bestaat, wordt het gemaakt voor u. 

Als u wilt gebruiken AzCopy, open een opdrachtprompt op uw lokale computer en Ga naar de map waarin AzCopy is geïnstalleerd. Dit is vergelijkbaar met *C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*. 

Alle bestanden in een container wilt kopiëren, gebruikt u de schakeloptie **/S** . Dit kan worden gebruikt om de VHD OS en alle gegevensschijven kopiëren als ze zich in dezelfde container. In dit voorbeeld wordt gedemonstreerd hoe alle bestanden in de container **mysourcecontainer** in opslag account **mysourcestorageaccount** kopiëren naar de container **mydestinationcontainer** in het **mydestinationstorageaccount** opslag-account. Vervang de namen van de opslagruimte accounts en containers door uw eigen. Vervang `<sourceStorageAccountKey1>` en `<destinationStorageAccountKey1>` met uw eigen toetsen.

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

Als u alleen kopiëren van een specifieke VHD in een container met meerdere bestanden wilt, kunt u ook de naam van het bestand met de schakeloptie /Pattern opgeven. In dit voorbeeld wordt alleen het bestand met de naam **myFileName.vhd** worden gekopieerd.

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
        /Pattern:myFileName.vhd
```


Wanneer deze is voltooid, krijgt u een bericht dat ziet er ongeveer zo uit:

```
  Finished 2 of total 2 file(s).
  [2016/10/07 17:37:41] Transfer summary:
  -----------------
  Total files transferred: 2
  Transfer successfully:   2
  Transfer skipped:        0
  Transfer failed:         0
  Elapsed time:            00.00:13:07
```

## <a name="troubleshooting"></a>Problemen oplossen

- Wanneer u AZCopy, gebruiken als u ziet dat de fout 'Server kan niet worden geverifieerd het verzoek. Zorg ervoor dat de waarde van koptekst verificatie wordt gevormd correct inclusief de handtekening." en u gebruikt een 2-toets of de secundaire opslag-toets, probeer met de toets primaire of 1e opslag.


## <a name="next-steps"></a>Volgende stappen

- U kunt een nieuwe VM maken door [de kopie van de VHD voor een VM als een schijf OS koppelen](virtual-machines-windows-create-vm-specialized.md).












