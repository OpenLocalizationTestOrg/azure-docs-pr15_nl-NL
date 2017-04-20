<properties
    pageTitle="Een afbeelding maken van een VM van de Windows Azure | Microsoft Azure"
    description="Een afbeelding van een Windows Azure virtuele machine die zijn gemaakt met het implementatiemodel klassieke maken."
    services="virtual-machines-windows"
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
    ms.date="09/27/2016"
    ms.author="cynthn"/>

#<a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Een afbeelding van een Windows Azure virtuele machine die zijn gemaakt met het implementatiemodel klassieke maken.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zie [Windows VM kopie maken in Azure uitgevoerd](virtual-machines-windows-vhd-copy.md)voor resourcemanager model informatie.


In dit artikel leest u hoe u het vastleggen van een Azure virtuele machines waarop Windows wordt uitgevoerd, zodat u deze als een afbeelding gebruiken kunt andere virtuele machines maken. In deze afbeelding bevat de schijf besturingssysteem en gegevensschijven die zijn gekoppeld aan de virtuele machine. Deze bevat geen netwerken configuraties, zodat u configureren die moet bij het maken van de andere virtuele machines waarop de afbeelding.

Azure wordt de afbeelding onder **Mijn afbeeldingen**opgeslagen. Dit is dezelfde plaats waar u hebt geüpload afbeeldingen worden opgeslagen. Zie voor meer informatie over afbeeldingen, [over afbeeldingen voor virtuele machines](virtual-machines-linux-classic-about-images.md).

##<a name="before-you-begin"></a>Voordat u begint##

Deze stappen wordt ervan uitgegaan dat u hebt al een Azure virtuele machine gemaakt en geconfigureerd van het besturingssysteem, inclusief eventuele gegevensschijven koppelen. Als u dit nog niet hebt gedaan, raadpleegt u deze instructies:

- [Een virtuele machine maken van een afbeelding.](virtual-machines-windows-classic-createportal.md)
- [Hoe u een gegevensschijf koppelt aan een virtuele machine](virtual-machines-windows-classic-attach-disk.md)
- Zorg ervoor dat de serverrollen worden ondersteund met Sysprep. Zie [Sysprep-ondersteuning voor serverrollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)voor meer informatie.

> [AZURE.WARNING] Dit proces wordt de oorspronkelijke virtuele machine verwijderd nadat deze vastgelegd. 

Voordat u een afbeelding van een Azure virtuele machines caputuring, is het raadzaam de doeltoepassing virtuele machine back-up worden gemaakt. Azure virtuele machines kunt worden back-up gemaakt met Azure back-up. Zie [een Back-up Azure virtuele machines](../backup/backup-azure-vms.md)voor meer informatie. Andere oplossingen zijn beschikbaar op gecertificeerde partners. Als u wilt weten wat momenteel beschikbaar is, zoek de Azure Marketplace.


##<a name="capture-the-virtual-machine"></a>De virtuele machine vastleggen

1. Klik in de [portal van Azure klassieke](http://manage.windowsazure.com) **verbinding maken** met de virtuele machine. Zie voor instructies [hoe u aanmelden bij een virtuele machine met Windows Server] [].

2.  Open een opdrachtpromptvenster als beheerder.

3.  Activeer de map `%windir%\system32\sysprep`, en Voer sysprep.exe.

4.  Het dialoogvenster **Hulpprogramma voor het voorbereiden van systeem** wordt weergegeven. Ga als volgt:

    - In het **Systeem opruimen actie**, selecteer **systeem Voer kant-en-klare Experience (OOBE)** en controleert u of **Generalize** is ingeschakeld. Zie voor meer informatie werken [hoe u gebruik Sysprep: een inleiding][].

    - Selecteer bij **Opties voor afsluiten** **Afsluiten**.

    - Klik op **OK**.

    ![Sysprep uitvoeren](./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png)

7.  Sysprep is afgesloten van de virtuele machine, waardoor de status van de virtuele machine in de portal van Azure klassieke in **gestopt wijzigt**.

8.  Klik in de klassieke portal Azure **virtuele Machines** op en selecteer de virtuele machine die u wilt vastleggen.

9.  Klik op de balk met opdrachten op **vastleggen**.

    ![VM vastleggen](./media/virtual-machines-windows-classic-capture-image/CaptureVM.png)

    Het dialoogvenster voor het **vastleggen van de virtuele Machine** wordt weergegeven.

10. Typ een naam voor de nieuwe afbeelding in het vak **De naam van de afbeelding**.

11. Voordat u een Windows Server-afbeelding aan een set met aangepaste afbeeldingen toevoegen, moet u deze algemene Sysprep volgens de instructies in de vorige stappen uitvoert. Klik op **ik Sysprep op de virtuele machine hebt uitgevoerd** om aan te geven u dat hebt gedaan.

12. Klik op het vinkje vastleggen van de afbeelding. De nieuwe afbeelding is nu beschikbaar onder **afbeeldingen**.

    ![Afbeelding van de opname geslaagd](./media/virtual-machines-windows-classic-capture-image/VMCapturedImageAvailable.png)

##<a name="next-steps"></a>Volgende stappen

De afbeelding is klaar voor het maken van virtuele machines worden gebruikt. Klik hiertoe maakt u een virtuele machine met behulp van het menu-item **Uit de galerie** en selecteren van de afbeelding die u zojuist hebt gemaakt. Zie voor instructies voor het [maken een virtuele machine uit een afbeelding](virtual-machines-windows-classic-createportal.md).



[Hoe kan ik me aanmelden een virtuele machine met Windows Server]: virtual-machines-windows-classic-connect-logon.md
[Het gebruik van Sysprep: een inleiding]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png
[The virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of the virtual machine]: ./media/virtual-machines-windows-classic-capture-image/CaptureVM.png
[Enter the image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use the captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png
