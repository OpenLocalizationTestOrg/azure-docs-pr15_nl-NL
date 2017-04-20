<properties
    pageTitle="Een schijf hebt toegevoegd aan een VM | Microsoft Azure"
    description="Een gegevensschijf toevoegen aan een virtuele Windows-computer die zijn gemaakt met het implementatiemodel klassieke en geïnitialiseerd."
    services="virtual-machines-windows, storage"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="cynthn"/>

# <a name="attach-a-data-disk-to-a-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Een gegevensschijf toevoegen aan een virtuele Windows-computer die zijn gemaakt met het implementatiemodel klassieke

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Als u de nieuwe portal gebruiken wilt, raadpleegt u [hoe u een gegevensschijf voor een Windows-VM in de portal van Azure hebt toegevoegd](virtual-machines-windows-attach-disk-portal.md).

Als u een schijf aanvullende gegevens nodig hebt, kunt u een lege schijf of een bestaande schijf met gegevens toevoegen aan een VM. In beide gevallen zijn de schijven VHD-bestanden die zich in een Azure opslag-account bevinden. In het geval van een nieuwe schijf, nadat u de schijf koppelen moet u ook geïnitialiseerd zodat de werkmap gereed voor gebruik door een Windows-VM is.

Zie voor meer informatie over schijven [over schijven en VHD's voor virtuele Machines](virtual-machines-windows-about-disks-vhds.md).


[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

## <a name="initialize-the-disk"></a>De schijf geïnitialiseerd

1. Verbinding maken met de virtuele machine. Zie voor instructies [hoe u zich aanmeldt bij een virtuele machine met Windows Server][logon].

2. Nadat u zich bij de virtuele machine aanmeldt, open **Serverbeheer**. Selecteer in het linkerdeelvenster **Bestands- en Storage-Services**.

    ![Serverbeheer openen](./media/virtual-machines-windows-classic-attach-disk/fileandstorageservices.png)

3. Het optiemenu uitvouwen en selecteer **schijven**.

4. De sectie **schijven** lijsten de schijven. In de meeste gevallen heeft dit schijf 0, schijf 1 en 2 schijf. Schijf 0 is de schijf besturingssysteem, schijf 1 is de tijdelijke schijf en schijf 2 is de gegevensschijf die u zojuist hebt gekoppeld aan de VM. De nieuwe gegevensschijf wordt de Partition vermeld als **Onbekend**. Met de rechtermuisknop op de schijf en selecteer **geïnitialiseerd**.

5.  U bent een melding dat alle gegevens worden gewist wanneer de schijf is geïnitialiseerd. Klik op **Ja** om te Bevestig de waarschuwing en initialisatie van de schijf. Als voltooid, wordt de Partion staan vermeld als **GUID**. Opnieuw met de rechtermuisknop op de schijf en selecteer **NieuwVolume**.

6.  Voltooi de wizard met de standaardwaarden. Wanneer de wizard is voltooid, wordt in de sectie **Volumes** het nieuwe volume bevat. De schijf is nu online en klaar voor de opslag van gegevens.

    ![Volume geïnitialiseerd](./media/virtual-machines-windows-classic-attach-disk/newvolumecreated.png)

> [AZURE.NOTE] De grootte van de VM bepaalt hoeveel schijven u aan deze kunt koppelen. Zie [grootten voor virtuele machines](virtual-machines-linux-sizes.md)voor meer informatie.

## <a name="additional-resources"></a>Aanvullende informatie

[Het ontkoppelen van een schijf uit een virtuele Windows-computer](virtual-machines-windows-classic-detach-disk.md)

[Over schijven en VHD's voor virtuele machines](virtual-machines-linux-about-disks-vhds.md)

[logon]: virtual-machines-windows-classic-connect-logon.md
