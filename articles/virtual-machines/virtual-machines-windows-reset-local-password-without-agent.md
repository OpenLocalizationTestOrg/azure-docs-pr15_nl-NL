<properties
   pageTitle="Een lokale Windows-wachtwoord opnieuw instellen als gast van Azure-agent is niet geïnstalleerd | Microsoft Azure"
   description="Hoe u het wachtwoord van een lokale Windows-gebruikersaccount opnieuw instellen als de Azure Gast-agent niet geïnstalleerd of op een VM functioneel is"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/05/2016"
   ms.author="iainfou"/>

# <a name="how-to-reset-local-windows-password-for-azure-vm"></a>Het wachtwoord opnieuw in te lokale Windows Azure VM
U kunt het lokale Windows-wachtwoord van een VM in Azure met behulp van de [Azure portal of Azure PowerShell](virtual-machines-windows-reset-rdp.md) dat de Azure Gast-agent is geïnstalleerd herstellen. Deze methode is de primaire manier om een wachtwoord opnieuw instellen voor een VM Azure. Als u problemen ondervindt met de Azure Gast-agent reageert niet, of niet worden geïnstalleerd na het uploaden van een aangepaste afbeelding, u handmatig kunt opnieuw een Windows-wachtwoord in. Dit artikel wordt uitgelegd hoe u een lokale wachtwoord opnieuw instellen door de bron OS virtuele schijf koppelen aan een andere VM. 

> [AZURE.WARNING] Gebruik deze procedure alleen noodgevallen. Probeer altijd een via de [portal van Azure of Azure PowerShell](virtual-machines-windows-reset-rdp.md) eerst wachtwoord opnieuw in te stellen.


## <a name="overview-of-the-process"></a>Overzicht van het proces
De stappen core voor de uitvoering van een lokale wachtwoord opnieuw instellen voor een Windows-VM in Azure wordt aangegeven wanneer er geen toegang tot de Azure Gast-agent is is als volgt:

- De bron VM verwijderen. Het virtuele schijven worden bewaard.
- De bron-VM OS schijf toevoegen aan een andere VM binnen uw Azure-abonnement. Deze VM is de probleemoplossing VM genoemd.
- Met het oplossen van problemen VM, maakt u enkele configuratiebestanden van de bron-VM OS schijf.
- Loskoppelen van de VM OS schijf van de probleemoplossing VM.
- Een sjabloon resourcemanager gebruiken om te maken van een VM, met de oorspronkelijke virtuele schijf.
- Wanneer de nieuwe VM opstart, werk configuratiebestanden die u maakt het wachtwoord van de vereiste gebruiker.


## <a name="detailed-steps"></a>Gedetailleerde stappen
Probeer altijd een via de [portal van Azure of Azure PowerShell](virtual-machines-windows-reset-rdp.md) voordat u probeert de volgende stappen wachtwoord opnieuw in te stellen. Zorg ervoor dat er een back-up van uw VM voordat u begint. 

1. Verwijder de desbetreffende VM in Azure-portal. De VM verwijderen verwijdert u alleen de metagegevens, de verwijzing naar de VM binnen Azure. Het virtuele schijven blijven behouden wanneer de VM wordt verwijderd:

    - Selecteer de VM in de portal van Azure, klikt u op *verwijderen*:

    ![Bestaande VM verwijderen](./media/virtual-machines-windows-reset-local-password-without-guest-agent/delete_vm.png)

2. De bron-VM OS schijf voor het oplossen van problemen VM hebt toegevoegd. Het oplossen van problemen VM moet zich in dezelfde regio als de bron-VM OS schijf (zoals `West US`):

    - Selecteer de probleemoplossing VM in de portal van Azure. Klik op *schijven* | *bijvoegen bestaande*:

    ![Bestaande schijf bijvoegen](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_attach_existing.png)

    Selecteer *VHD-bestand* en selecteer vervolgens het opslag-account met uw gegevensbron VM:

    ![Selecteer opslag-account](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_storageaccount.PNG)

    Selecteer de Broncontainer. De Broncontainer is meestal *VHD's*:

    ![Selecteer de container opslag](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_container.png)

    Selecteer de vhd OS om te koppelen. Klik op *selecteren* om het proces te voltooien:

    ![Selecteer de virtuele bronschijf](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_source_vhd.png)

3. Verbinding maken met het oplossen van problemen VM met extern bureaublad en controleer of dat de bron-VM OS schijf zichtbaar is:

    - Het oplossen van problemen VM in de portal van Azure en klik op *verbinding maken*.
    - Open het RDP-bestand dat is gedownload. Voer de gebruikersnaam en wachtwoord van het oplossen van problemen VM.
    - Zoek in Verkenner naar de gegevensschijf die u bijgevoegd. Als de bron van VM VHD de schijf met alleen gegevens die zijn bijgevoegd bij het oplossen van problemen VM is, moet het station F::

    ![Gekoppelde gegevensschijf bekijken](./media/virtual-machines-windows-reset-local-password-without-guest-agent/troubleshooting_vm_fileexplorer.png)

4. Maak `gpt.ini` in `\Windows\System32\GroupPolicy` op de bron-VM station (als gpt.ini bestaat, wijzig in gpt.ini.bak):

    > [AZURE.WARNING] Zorg ervoor dat u niet per ongeluk Maak de volgende bestanden in C:\Windows, het station OS voor het oplossen van problemen VM. De volgende bestanden in het station OS voor uw gegevensbron VM die is gekoppeld als een gegevensschijf maken.

    - Voeg de volgende regels in de `gpt.ini` bestand dat u hebt gemaakt:

    ```
    [General]
    gPCFunctionalityVersion=2
    gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
    Version=1
    ```

    ![Gpt.ini maken](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_gpt_ini.png)
 
5. Maak `scripts.ini` in `\Windows\System32\GroupPolicy\Machine\Scripts`. Controleer of verborgen mappen worden weergegeven. Maak indien nodig de `Machine` of `Scripts` mappen.

    - Voeg de volgende regels de `scripts.ini` bestand dat u hebt gemaakt:

    ```
    [Startup]
    0CmdLine=C:\Windows\System32\FixAzureVM.cmd
    0Parameters=
    ```

    ![Scripts.ini maken](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_scripts_ini.png)
 
6. Maak `FixAzureVM.cmd` in `\Windows\System32` met de volgende inhoud, vervangen `<username>` en `<newpassword>` met uw eigen waarden:

    ```
    NET USER <username> <newpassword>
    ```

    ![FixAzureVM.cmd maken](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_fixazure_cmd.png)

    U kunt de geconfigureerde complexiteit van het wachtwoordvereisten voor uw VM moet voldoen aan bij het definiëren van het nieuwe wachtwoord.

7. Ontkoppel de schijf van de probleemoplossing VM in Azure-portal:

    - Selecteer de probleemoplossing VM in de portal van Azure, klikt u op *schijven*.
    - Selecteer de gegevensschijf die zijn toegevoegd in stap 2, klikt u op *ontkoppelen*:

    ![Schijf loskoppelen](./media/virtual-machines-windows-reset-local-password-without-guest-agent/detach_disk.png)

8. Voordat u een VM maken, verkrijgt u de URI naar de bron OS schijf:

    - Selecteer het account waarmee opslag in de portal van Azure, klikt u op *BLOB's*.
    - Selecteer de container. De Broncontainer is meestal *VHD's*:

    ![Selecteer account-blob storage](./media/virtual-machines-windows-reset-local-password-without-guest-agent/select_storage_details.png)

    Selecteer de bron VM OS VHD en klik op de knop *kopiëren* naast de *URL* -naam:

    ![Schijf URI kopiëren](./media/virtual-machines-windows-reset-local-password-without-guest-agent/copy_source_vhd_uri.png)

9. Maak een VM uit de bron-VM OS schijf:

    - Gebruik [deze sjabloon Azure resourcemanager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) een VM vanuit een gespecialiseerde VHD maken. Klik op de `Deploy to Azure` knop te openen van de Azure-portal met de sjablonen details voor u ingevuld.
    - Als u bewaren van de vorige instellingen voor de VM wilt, selecteert u de *sjabloon bewerken* op te geven van uw bestaande VNet, subnet, netwerkadapter of openbare IP.
    - In de `OSDISKVHDURI` parameter tekstvak, de URI van uw gegevensbron VHD verkrijgen in de vorige stap plakken:

    ![Een VM van sjabloon maken](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_new_vm_from_template.png)

10. Nadat u de nieuwe VM wordt uitgevoerd, verbinding maken met de VM extern bureaublad gebruiken met het nieuwe wachtwoord dat u hebt opgegeven in de `FixAzureVM.cmd` script.

11. Verwijder de volgende bestanden om de omgeving op te schonen vanuit uw externe sessie voor het nieuwe VM:

    - Uit %windir%\System32
        - FixAzureVM.cmd verwijderen
    - Uit %windir%\System32\GroupPolicy\Machine\
        - scripts.ini verwijderen
    - Uit %windir%\System32\GroupPolicy
        - gpt.ini verwijderen (als gpt.ini vóór bevatte en u deze hebt gewijzigd in gpt.ini.bak, naam wijzigen het .bak-bestand terug naar gpt.ini)

## <a name="next-steps"></a>Volgende stappen
Als u nog steeds geen verbinding met extern bureaublad, raadpleegt u de [gids voor probleemoplossing voor RDP](virtual-machines-windows-troubleshoot-rdp-connection.md). De [gedetailleerde voor probleemoplossing RDP-handleiding](virtual-machines-windows-detailed-troubleshoot-rdp.md) analyseert probleemoplossing methoden in plaats van specifieke stappen. U kunt ook [open een ondersteuningsverzoek voor een Azure](https://azure.microsoft.com/support/options/) voor praktijkervaring hulp.