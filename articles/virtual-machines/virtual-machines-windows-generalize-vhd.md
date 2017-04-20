<properties
    pageTitle="Maak een Windows VHD algemeen | Microsoft Azure"
    description="Informatie over het gebruik van Sysprep generaliseren van een Windows-VM voor gebruik met het implementatiemodel resourcemanager."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>
    
    
    
    
# <a name="generalize-a-windows-virtual-machine-using-sysprep"></a>Een virtuele Windows-computer met Sysprep generalize

In dit gedeelte ziet u hoe u uw virtuele Windows-computer voor gebruik als een afbeelding generalize. Sysprep Hiermee verwijdert u alle persoonlijke gegevens van uw account, onder andere en zorgt ervoor dat de computer moet worden gebruikt als een afbeelding. Zie voor meer informatie over Sysprep, [hoe u gebruik Sysprep: een inleiding](http://technet.microsoft.com/library/bb457073.aspx).

Zorg ervoor dat de serverrollen uitgevoerd op de computer worden ondersteund door Sysprep. Zie voor meer informatie [Sysprep-ondersteuning voor serverrollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

>[AZURE.IMPORTANT] Als u Sysprep voordat u uw VHD uploadt naar Azure voor de eerste keer uitvoert, controleert u of u [uw VM voorbereid](virtual-machines-windows-prepare-for-upload-vhd-image.md) voordat u Sysprep uitvoert. 

1. Meld u aan bij de virtuele Windows-computer.

2. Open het opdrachtpromptvenster als beheerder. De map wijzigen in **%windir%\system32\sysprep**en voer vervolgens `sysprep.exe`.

3. Klik in het dialoogvenster **Hulpprogramma voor het voorbereiden van systeem** Selecteer **systeem Voer kant-en-klare Experience (OOBE)**en zorg ervoor dat het selectievakje **Generalize** is geselecteerd.

4. Selecteer bij **Opties voor afsluiten** **Afsluiten**.

5. Klik op **OK**.

    ![Sysprep starten](./media/virtual-machines-windows-upload-image/sysprepgeneral.png)

6. Als Sysprep is voltooid, wordt deze afgesloten de virtuele machine. 

## <a name="next-steps"></a>Volgende stappen

- Als de VM on-premises implementatie is, kunt u nu [de VHD naar Azure uploaden](virtual-machines-windows-upload-image.md).
- Als de VM al in Azure is, kunt u nu [een afbeelding van de algemene VM maken](virtual-machines-windows-capture-image.md).