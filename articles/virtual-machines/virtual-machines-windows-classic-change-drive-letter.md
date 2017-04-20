<properties
    pageTitle="Controleer het station D: van een VM een gegevensschijf | Microsoft Azure"
    description="Beschrijving van het stationsletters voor een Windows-VM wijzigt, zodat u het station D: als een gegevensstation gebruiken kunt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="use-the-d-drive-as-a-data-drive-on-a-windows-vm"></a>Het station D: gebruiken als een gegevensstation op een Windows-VM 

Als uw toepassing het station D gebruiken moet voor de opslag van gegevens, volgt u deze instructies om te gebruiken van een andere letter voor de tijdelijke schijf. Gebruik nooit de tijdelijke schijf voor de opslag van gegevens die u wilt bewaren.

Als u het formaat wijzigen of **stoppen (Deallocate)** een virtuele machine, kan dit plaatsing van de virtuele machine naar een nieuwe hypervisor activeren. Een onderhoudsgebeurtenis of niet gepland kan ook zo activeren dat deze positie. In dit scenario wordt de tijdelijke schijf toegewezen aan de eerste beschikbare letter. Als u een toepassing die specifiek is vereist het station D: hebt, moet u als volgt te werk als u wilt tijdelijk pagefile.sys verplaatsen, een nieuwe gegevensschijf hebt toegevoegd en dit de letter D toewijzen en vervolgens pagefile.sys terug naar het tijdelijke station verplaatsen. Als voltooid, wordt Azure niet overnemen D: als de VM drukt, naar een ander hypervisor verplaatst.

Zie voor meer informatie over het gebruik van de tijdelijke schijf in Azure, [informatie over het tijdelijk station op Microsoft Azure virtuele Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="attach-the-data-disk"></a>De gegevensschijf koppelen

Eerst moet u de gegevensschijf toevoegen aan de virtuele machine. 

- Als u wilt gebruiken in de portal, raadpleegt u [het toevoegen van een gegevensschijf in de portal van Azure](virtual-machines-windows-attach-disk-portal.md)
- Als u wilt gebruiken in de klassieke-portal, Zie [hoe u een gegevensschijf aan een virtuele Windows-computer als bijlage toevoegen](virtual-machines-windows-classic-attach-disk.md). 


## <a name="temporarily-move-pagefilesys-to-c-drive"></a>Tijdelijk verplaatsen pagefile.sys naar station C

1. Verbinding maken met de virtuele machine. 

2. Met de rechtermuisknop op het menu **Start** en de optie **systeem**.

3. Selecteer in het menu linkerkant **Geavanceerde systeeminstellingen**.

4. Selecteer **Instellingen**in de sectie **prestaties** .

5. Selecteer het tabblad **Geavanceerd** .

5. Selecteer in de sectie **virtueel geheugen** **wijzigen**.

6. Selecteer het station **C** en klik vervolgens op **systeem beheerde grootte** en klik vervolgens op **instellen**.

7. Selecteer het station **D** en klik vervolgens op **geen paginering-bestand** en klik vervolgens op **instellen**.

8. Klik op toepassen. Hier krijgt u een waarschuwing dat de computer moet opnieuw worden gestart voor de wijzigingen door te voeren.

9. Start opnieuw op de virtuele machine.




## <a name="change-the-drive-letters"></a>De letters wijzigen 

1. Zodra de VM opnieuw is opgestart, meld u weer aan bij de VM.

2. Klik op het menu **Start** en typ **diskmgmt.msc** en druk op Enter. Schijfbeheer van de wordt gestart.

3. Met de rechtermuisknop op **D**, het station tijdelijke opslag, en selecteer **wijzigen stationsletter en paden**.

4. Klik onder station brief, selecteer station **G** en klik vervolgens op **OK**. 

5. Met de rechtermuisknop op de gegevensschijf en selecteer **station wijzigen en paden**.

6. Klik onder station brief, selecteer station **D** en klik vervolgens op **OK**. 

7. Met de rechtermuisknop op **G**, het station tijdelijke opslag, en selecteer **wijzigen stationsletter en paden**.

8. Klik onder station brief, selecteer station **E** en klik vervolgens op **OK**. 

> [AZURE.NOTE] Als uw VM andere schijven of -stations bevat, kunt u dezelfde methode gebruiken om toe te wijzen de stationsletters van de andere schijven en stations. U wilt dat de configuratie van de schijven moet zijn:  
>- C: OS Schijfopruiming  
>- D: gegevens Schijfopruiming  
>- E-tijdelijke schijf



## <a name="move-pagefilesys-back-to-the-temporary-storage-drive"></a>Pagefile.sys terug verplaatsen naar het station tijdelijke opslag 

1. Met de rechtermuisknop op het menu **Start** en selecteer **systeem**

2. Selecteer in het menu linkerkant **Geavanceerde systeeminstellingen**.

3. Selecteer **Instellingen**in de sectie **prestaties** .

4. Selecteer het tabblad **Geavanceerd** .

5. Selecteer in de sectie **virtueel geheugen** **wijzigen**.

6. Selecteer het OS station **C** en klik op **geen paginering-bestand** en klik op **instellen**.

7. Selecteer het station tijdelijke **E** en klik vervolgens op **systeem beheerde grootte** en klik op **instellen**.

8. Klik op **toepassen**. Hier krijgt u een waarschuwing dat de computer moet opnieuw worden gestart voor de wijzigingen door te voeren.

9. Start opnieuw op de virtuele machine.




## <a name="next-steps"></a>Volgende stappen
- U kunt de opslagruimte beschikbaar vergroten naar uw VM door te [koppelen van een schijf aanvullende gegevens](virtual-machines-windows-attach-disk-portal.md).



