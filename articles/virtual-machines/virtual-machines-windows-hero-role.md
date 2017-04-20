<properties
    pageTitle="IIS installeren op uw eerste Windows VM | Microsoft Azure"
    description="Experimenteren met uw eerste virtuele Windows-computer door IIS en openen van poort 80 met behulp van de Azure portal te installeren."
    keywords=""
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
    ms.date="09/06/2016"
    ms.author="cynthn"/>

# <a name="experiment-with-installing-a-role-on-your-windows-vm"></a>Experimenteren met de installatie van een rol op uw Windows-VM
    
Nadat u uw eerste VM (virtual machine) snel hebt, kunt u op verplaatsen naar het installeren van software en services. Voor deze zelfstudie gaan we Serverbeheer gebruiken op de Windows Server VM IIS installeren. We maakt vervolgens een netwerk beveiliging groep (NSG) met behulp van de Azure portal poort 80 naar IIS-verkeer door te openen. 

Als u uw eerste VM al hebt gemaakt, moet u teruggaan naar [uw eerste virtuele Windows-computer in de portal van Azure maken](virtual-machines-windows-hero-tutorial.md) voordat u verdergaat met deze zelfstudie.

## <a name="make-sure-the-vm-is-running"></a>Controleer of dat de VM wordt uitgevoerd

1. Open de [portal van Azure](https://portal.azure.com).
2. Klik in het menu hub op **virtuele Machines**. Selecteer de virtuele machine in de lijst.
3. Als de status ervan is **gestopt (Deallocated)**, klikt u op de knop **Start** op het blad **Essentials** van VM. Als de status ervan **uitgevoerd is**, kunt u op verplaatsen naar de volgende stap.

## <a name="connect-to-the-virtual-machine-and-sign-in"></a>Verbinding maken met de virtuele machine en u aanmelden

1.  Klik in het menu hub op **virtuele Machines**. Selecteer de virtuele machine in de lijst.

3. Klik op het blad voor de virtuele machine, klikt u op **verbinding maken**. Hiermee wordt gemaakt en downloads van een bestand met Remote Desktop Protocol (RDP-bestand) dat lijkt op een snelkoppeling verbinding maken met uw computer. U wilt mogelijk Sla het bestand op uw bureaublad voor eenvoudige toegang. **Open** dit bestand verbinding maken met uw VM.

    ![Schermafbeelding van de Azure-portal laat zien hoe u verbinding maken met uw VM](./media/virtual-machines-windows-hero-tutorial/connect.png)

4. U krijgt een waarschuwing weergegeven dat de RDP afkomstig van een onbekende uitgever is. Dit is normaal. Klik op **verbinding maken** om door te gaan in het venster extern bureaublad.

    ![Schermafbeelding van een waarschuwing over een onbekende uitgever](./media/virtual-machines-windows-hero-tutorial/rdp-warn.png)

5. Typ de gebruikersnaam en wachtwoord voor het lokale account die u hebt gemaakt wanneer u de VM gemaakt in het venster Windows-beveiliging. De gebruikersnaam wordt ingevoerd als *vmname*& #92; *gebruikersnaam*, klikt u op **OK**.

    ![Schermafbeelding van de VM naam, gebruikersnaam en wachtwoord in te voeren](./media/virtual-machines-windows-hero-tutorial/credentials.png)
    
6.  U er een waarschuwing weergegeven dat het certificaat kan niet worden gecontroleerd. Dit is normaal. Klik op **Ja** om te controleren of de identiteit van de virtuele machine en klaar bent met het aanmelden.

    ![Schermafbeelding van een bericht, maar de identiteit van de VM](./media/virtual-machines-windows-hero-tutorial/cert-warning.png)


Als u uitvoert in om problemen op wanneer u probeert om verbinding te maken, raadpleegt u [problemen met extern bureaublad verbindingen naar een Windows Azure virtuele Machine](virtual-machines-windows-troubleshoot-rdp-connection.md).


## <a name="install-iis-on-your-vm"></a>IIS installeren op uw VM

Nu dat u bent aangemeld voor VM, wordt we een functie van de server installeren, zodat u meer kunt experimenteren.

1. Open **Serverbeheer** als dit nog niet is geopend. Klik op het menu **Start** en klik vervolgens op **Serverbeheer**.
2. Selecteer in **Serverbeheer** **Lokale Server** in het linkerdeelvenster. 
3. Selecteer in het menu **beheren** > **rollen toevoegen en functies**.
4. In de Wizard van de functies, klik op de pagina **Installatietype** en rollen toevoegen **op basis van rollen of functies gebaseerde installatie**kiezen en klik vervolgens op **volgende**.

    ![Schermafbeelding van het tabblad rollen toevoegen en Wizard onderdelen voor installatietype](./media/virtual-machines-windows-hero-tutorial/role-wizard.png)

5. Selecteer de VM uit de groep server en klik op **volgende**.
6. Selecteer op de pagina **Serverrollen** **Webserver (IIS)**.

    ![Schermafbeelding van het tabblad rollen toevoegen en Wizard onderdelen voor serverrollen](./media/virtual-machines-windows-hero-tutorial/add-iis.png)

7. Zorg ervoor dat de **Hulpmiddelen voor projectbeheer opnemen** is geselecteerd en klik vervolgens op **Onderdelen toevoegen**in het pop-upvenster over het toevoegen van functies die u nodig hebt voor IIS. Wanneer het pop-upvenster wordt gesloten, klikt u op **volgende** in de wizard.

    ![Schermafbeelding van de pop-om te bevestigen de rol IIS toevoegen](./media/virtual-machines-windows-hero-tutorial/confirm-add-feature.png)

8. Klik op **volgende**op de pagina onderdelen.
9. Klik op **volgende**op de pagina **Webserver rol (IIS)** . 
10. Klik op **volgende**op de pagina **Rolservices** . 
11. Klik op **de bevestigingspagina** op **installeren**. 
12. Wanneer de installatie is voltooid, klikt u op **sluiten** in de wizard.



## <a name="open-port-80"></a>Open poort 80 

In de volgorde voor uw VM accepteren binnenkomende verkeer via poort 80, moet u een binnenkomende regel toevoegen aan de beveiligingsgroep van het netwerk. 

1. Open de [portal van Azure](https://portal.azure.com).
2. Selecteer in de **virtuele machines** de VM die u hebt gemaakt.
3. In de virtuele machines-instellingen, selecteer **netwerk-interfaces** en selecteer vervolgens de bestaande netwerkinterface.

    ![Schermafbeelding van de instelling VM voor de netwerkinterfaces](./media/virtual-machines-windows-hero-tutorial/network-interface.png)

4. In **Essentials** voor de network interface, klikt u op de **beveiligingsgroep van netwerk**.

    ![Schermafbeelding van de sectie Essentials voor de network interface](./media/virtual-machines-windows-hero-tutorial/select-nsg.png)

5. Klik in het blad **Essentials** voor de NSG, moet u een bestaande standaard binnenkomende regel voor **standaard-toestaan-rdp** waarmee u zich aanmelden bij de VM hebben. U kunt een andere binnenkomende regel zodat IIS-verkeer wordt toegevoegd. Klik op **regel voor binnenkomende**.

    ![Schermafbeelding van de sectie Essentials voor de NSG](./media/virtual-machines-windows-hero-tutorial/inbound.png)

6. In **inkomende beveiligingsregels**en klikt u op **toevoegen**.

    ![Schermafbeelding met de knop voor het toevoegen van een regel](./media/virtual-machines-windows-hero-tutorial/add-rule.png)

7. In **inkomende beveiligingsregels**en klikt u op **toevoegen**. **80** Typ in het poortbereik en zorg ervoor dat **toestaan** is geselecteerd. Wanneer u klaar bent, klikt u op **OK**.

    ![Schermafbeelding met de knop voor het toevoegen van een regel](./media/virtual-machines-windows-hero-tutorial/port-80.png)
 
Zie voor meer informatie over NSGs, regels voor binnenkomende en uitgaande [toestaan externe toegang tot uw VM met behulp van de Azure portal](virtual-machines-windows-nsg-quickstart-portal.md)
 
## <a name="connect-to-the-default-iis-website"></a>Verbinding maken met de standaard IIS-website

1. Klik in de portal Azure **virtuele machines** op en selecteer vervolgens uw VM.
2. Kopieer in het blad **Essentials** uw **openbare IP-adres**.

    ![Schermafbeelding die laat zien waar vind ik het openbare IP-adres van uw VM](./media/virtual-machines-windows-hero-tutorial/ipaddress.png)

2. Open een browser en typ in de adresbalk in uw openbare IP-adres als volgt: http://<publicIPaddress> en klikt u op **Enter** om naar dit adres te gaan.
3. Uw browser, moet de standaard IIS-webpagina openen. Deze er ongeveer zo uitziet:

    ![Schermafbeelding die laat zien hoe de standaardpagina IIS eruitziet in een browser](./media/virtual-machines-windows-hero-tutorial/iis-default.png)

    

## <a name="next-steps"></a>Volgende stappen

- Ook kunt u experimenteren met [een gegevensschijf koppelen](virtual-machines-windows-attach-disk-portal.md) aan uw virtuele machine. Gegevensschijven bieden meer opslagruimte voor uw virtuele machine.
