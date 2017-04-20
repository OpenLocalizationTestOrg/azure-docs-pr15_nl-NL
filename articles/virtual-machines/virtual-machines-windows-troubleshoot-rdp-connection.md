<properties
    pageTitle="Kan geen RDP voor een Azure VM | Microsoft Azure"
    description="Problemen oplossen wanneer u geen verbinding maken met uw Windows virtuele machine in Azure wordt aangegeven met extern bureaublad"
    keywords="Externe bureaublad fout, verbinding met extern bureaublad fout, kan geen verbinding met VM, probleemoplossing extern bureaublad"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="10/26/2016"
    ms.author="iainfou"/>

# <a name="troubleshoot-remote-desktop-connections-to-an-azure-virtual-machine"></a>Problemen met extern bureaublad-verbindingen met een Azure virtuele machines

De Remote Desktop Protocol (RDP)-verbinding met uw Windows Azure virtuele machine (VM) kan om verschillende redenen, zodat u geen toegang tot uw VM mislukken. Het probleem is met de service Extern bureaublad in de VM, de netwerkverbinding of op de client extern bureaublad op uw hostcomputer. In dit artikel begeleidt u bij enkele van de meest voorkomende methoden RDP verbindingsproblemen op te lossen. 

Als u meer hulp op een willekeurige plaats in dit artikel nodig hebt, kunt u contact opnemen met de Azure experts op [het MSDN Azure en stapel overloop forums](https://azure.microsoft.com/support/forums/). U kunt ook een incident Azure ondersteuning indienen. Ga naar de [Azure ondersteunen site](https://azure.microsoft.com/support/options/) en selecteer **Ondersteuning krijgen**.

<a id="quickfixrdp"></a>

## <a name="quick-troubleshooting-steps"></a>Snelle stappen voor probleemoplossing
Na elke stap bij probleemoplossing, probeert u opnieuw verbinding maken met de VM:

1. Opnieuw extern bureaublad configureren.
2. Netwerk beveiligingsgroep controleren regels / Cloud Services-eindpunten.
3. VM console logboeken bekijken.
4. De status van de Resource VM controleren.
5. Uw wachtwoord VM opnieuw instellen.
6. Start uw VM opnieuw.
7. Implementeer deze opnieuw uw VM.

Lees verder als u meer gedetailleerde stappen en uitleg nodig.

> [AZURE.TIP] Als de knop **verbinding maken** voor uw VM wordt grijs weergegeven in de portal en u geen verbinding is met Azure via een [Express Route](../expressroute/expressroute-introduction.md) of [Site-naar-Site VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) -verbinding, die u wilt maken en uw VM toewijzen een openbare IP-adres voordat u RDP kunt gebruiken. U vindt meer informatie over [openbare IP-adressen in Azure wordt aangegeven](../virtual-network/virtual-network-ip-addresses-overview-arm.md).


## <a name="ways-to-troubleshoot-rdp-issues"></a>Manieren waarop kunt RDP-problemen oplossen
U kunt VMs die zijn gemaakt met behulp van het implementatiemodel resourcemanager met behulp van een van de volgende manieren oplossen:

- [Azure-portal](#using-the-azure-portal) - uitstekende als u wilt snel opnieuw de RDP-configuratie of gebruikersreferenties en u hoeft niet de Azure's hebben geïnstalleerd.
- [Azure PowerShell](#using-azure-powershell) - als u vertrouwd met een PowerShell-prompt bent snel opnieuw instellen de RDP-configuratie of gebruiker referenties zijn de Azure PowerShell-cmdlets gebruiken.

U vindt ook stappen over het oplossen van VMs die zijn gemaakt met behulp van het [model Klassiek-implementatie](#troubleshoot-vms-created-using-the-classic-deployment-model).


<a id="fix-common-remote-desktop-errors"></a>
## <a name="troubleshoot-using-the-azure-portal"></a>Problemen met behulp van de Azure portal
Probeer opnieuw verbinding te maken voor uw VM na elke stap bij probleemoplossing. Als u nog steeds geen verbinding, kunt u de volgende stap.

1. **Opnieuw instellen van uw RDP-verbinding**. De configuratie van de RDP opnieuw instellen wanneer externe verbindingen zijn uitgeschakeld of Windows Firewall regels RDP, bijvoorbeeld blokkeren in deze stap bij probleemoplossing.

    Selecteer uw VM in de portal van Azure. Schuif omlaag in het deelvenster instellingen naar de sectie **ondersteuning + probleemoplossing** bijna onder aan de lijst. Klik op de knop **wachtwoord opnieuw instellen** . Stel de **modus** **alleen configuratie opnieuw** in te stellen en klik op de knop **bijwerken** :

    ![Beginwaarden van de RDP-configuratie in de portal van Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/reset-rdp.png)

2. **Regels voor netwerk-beveiligingsgroep verifiëren**. Deze stap wordt gecontroleerd of dat er een regel in uw netwerk-beveiligingsgroep toe te staan RDP-verkeer is toegestaan. De standaardpoort voor RDP is TCP-poort 3389. Een regel toe te staan RDP-verkeer mogelijk niet automatisch worden gemaakt wanneer u uw VM maakt.

    Selecteer uw VM in de portal van Azure. Klik op het **Netwerkinterfaces** vanuit het deelvenster instellingen.

    ![Weergave netwerkinterfaces voor een VM in Azure-portal](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-network-interfaces.png)

    Selecteer de netwerkinterface van uw in de lijst (Er is meestal slechts één):

    ![Selecteer van netwerkinterface in de portal van Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-interface.png)

    Selecteer **de beveiligingsgroep netwerk** om weer te geven van de beveiligingsgroep van netwerk dat is gekoppeld aan de netwerkinterface van uw:

    ![Netwerk-beveiligingsgroep selecteren in de portal van Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-nsg.png)

    Controleer of een binnenkomende regel waarmee RDP-verkeer op TCP-poort 3389 bestaat. Het volgende voorbeeld ziet u een geldige beveiligings-regel waarmee RDP-verkeer is toegestaan. U kunt zien `Service` en `Action` correct zijn geconfigureerd:

    ![Controleer of RDP NSG regel in de portal van Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/verify-nsg-rules.png)

    Als u nog geen een regel waarmee RDP-verkeer, [een beveiligingsgroep voor netwerk-regel maken](virtual-machines-windows-nsg-quickstart-portal.md). TCP-poort 3389 toestaan.

3. **Controleren VM opstarten diagnostische gegevens**. Deze stap bij probleemoplossing reviseert het VM console-Logboeken om te bepalen als de VM is worden gebruikt voor het melden van een probleem. Niet alle VMs hebben opstarten diagnostische hulpprogramma's die zijn ingeschakeld, dus het is mogelijk dat deze stap bij probleemoplossing optioneel.
    
    Bepaalde stappen voor probleemoplossing buiten het bereik van dit artikel zijn, maar kunnen duiden op een grotere probleem dat dat dit gevolgen RDP connectivity heeft is. Zie voor meer informatie over het reviseren van de console-logboeken en VM schermafbeelding, [Opstarten diagnostische hulpprogramma's voor VMs](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

4. **De status van de Resource VM controleren**. Er zijn geen bekende problemen met het Azure platform die van invloed op connectiviteit met de VM zijn mogelijk wordt gecontroleerd met deze stap bij probleemoplossing.

    Selecteer uw VM in de portal van Azure. Schuif omlaag in het deelvenster instellingen naar de sectie **ondersteuning + probleemoplossing** bijna onder aan de lijst. Klik op de knop **status van de Resource** . Een correct VM rapporten die **beschikbaar**:

    ![VM resource status in de portal van Azure controleren](./media/virtual-machines-windows-troubleshoot-rdp-connection/check-resource-health.png)

5. De **gebruikersreferenties opnieuw instellen**. Deze stap bij probleemoplossing weer het wachtwoord van een lokale beheerdersaccount ingesteld als u niet zeker weet of de referenties bent vergeten.

    Selecteer uw VM in de portal van Azure. Schuif omlaag in het deelvenster instellingen naar de sectie **ondersteuning + probleemoplossing** bijna onder aan de lijst. Klik op de knop **wachtwoord opnieuw instellen** . Controleer of dat de **modus** is ingesteld op het **wachtwoord opnieuw in** en voer uw gebruikersnaam en een nieuw wachtwoord. Klik tot slot op de knop **bijwerken** :

    ![De gebruikersreferenties in de portal van Azure opnieuw instellen](./media/virtual-machines-windows-troubleshoot-rdp-connection/reset-password.png)

6. **Start uw VM opnieuw**. Deze stap bij probleemoplossing kan de VM zelf heeft onderliggende problemen worden gecorrigeerd.

    Uw VM in de portal van Azure en klik op het tabblad **Overzicht** . Klik op de knop **opnieuw starten** :

    ![Start de VM opnieuw in de portal van Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/restart-vm.png)

7. **Implementeer deze opnieuw uw VM**. Deze stap bij probleemoplossing redeploys uw VM naar een andere host binnen Azure elk onderliggende platform- of netwerkproblemen worden gecorrigeerd.

    Selecteer uw VM in de portal van Azure. Schuif omlaag in het deelvenster instellingen naar de sectie **ondersteuning + probleemoplossing** bijna onder aan de lijst. Klik op de knop **Implementeer deze opnieuw** , en klik vervolgens op **Implementeer deze opnieuw**:

    ![Implementeer deze opnieuw de VM in de portal van Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/redeploy-vm.png)

    Nadat deze bewerking is voltooid, kortstondige schijfgegevens gaat verloren en dynamische IP-adressen die zijn gekoppeld aan de VM worden bijgewerkt.

Als u nog steeds problemen RDP Doe, u kunt [openen van een verzoek voor ondersteuning](https://azure.microsoft.com/support/options/) of Lees [meer gedetailleerde RDP voor probleemoplossing concepten en stappen](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-using-azure-powershell"></a>Problemen met Azure PowerShell oplossen
Als u nog niet gedaan, [installeren en configureren van de meest recente Azure PowerShell hebt](../powershell-install-configure.md).

Gebruik de volgende voorbeelden variabelen, zoals `myResourceGroup`, `myVM`, en `myVMAccessExtension`. Deze namen van variabelen en locaties vervangen door uw eigen waarden.

> [AZURE.NOTE] U opnieuw de gebruikersreferenties en de RDP-configuratie met behulp van de PowerShell-cmdlet [Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) . In de volgende voorbeelden `myVMAccessExtension` is een naam die u als onderdeel van het proces opgeeft. Als u de VMAccessAgent eerder hebt gewerkt, kunt u de naam van de bestaande extensie verkrijgen met behulp van `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` om te controleren van de eigenschappen van de VM. Als u wilt weergeven op de naam, kijkt u onder de sectie 'Extensions' van de uitvoer.

Probeer opnieuw verbinding te maken voor uw VM na elke stap bij probleemoplossing. Als u nog steeds geen verbinding, kunt u de volgende stap.

1. **Opnieuw instellen van uw RDP-verbinding**. De configuratie van de RDP opnieuw instellen wanneer externe verbindingen zijn uitgeschakeld of Windows Firewall regels RDP, bijvoorbeeld blokkeren in deze stap bij probleemoplossing.

    De RDP-verbinding op een VM met de naam in het volgende voorbeeld wordt hersteld `myVM` in de `WestUS` locatie en klik in de resourcegroep met de naam `myResourceGroup`:

    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location Westus -Name "myVMAccessExtension"
    ```

2. **Regels voor netwerk-beveiligingsgroep verifiëren**. Deze stap wordt gecontroleerd of dat er een regel in uw netwerk-beveiligingsgroep toe te staan RDP-verkeer is toegestaan. De standaardpoort voor RDP is TCP-poort 3389. Een regel toe te staan RDP-verkeer mogelijk niet automatisch worden gemaakt wanneer u uw VM maakt.

    Eerst alle configuratiegegevens toewijzen voor uw netwerk-beveiligingsgroep wilt de `$rules` variabele. Het volgende voorbeeld wordt opgehaald voor informatie over het netwerk-beveiligingsgroep met de naam `myNetworkSecurityGroup` in de resourcegroep naam `myResourceGroup`:

    ```powershell
    $rules = Get-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
        -Name "myNetworkSecurityGroup"
    ```

    Nu de regels die zijn geconfigureerd voor de beveiligingsgroep van dit netwerk weergeven. Controleer of u een regel bestaat om toe te staan TCP-poort 3389 voor binnenkomende verbindingen als volgt:

    ```powershell
    $rules.SecurityRules
    ```

    Het volgende voorbeeld ziet u een geldige beveiligings-regel waarmee RDP-verkeer is toegestaan. U kunt zien `Protocol`, `DestinationPortRange`, `Access`, en `Direction` correct zijn geconfigureerd:

    ```powershell
    Name                     : default-allow-rdp
    Id                       : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/default-allow-rdp
    Etag                     : 
    ProvisioningState        : Succeeded
    Description              : 
    Protocol                 : TCP
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : *
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 1000
    Direction                : Inbound
    ```

    Als u nog geen een regel waarmee RDP-verkeer, [een beveiligingsgroep voor netwerk-regel maken](virtual-machines-windows-nsg-quickstart-powershell.md). TCP-poort 3389 toestaan.

3. De **gebruikersreferenties opnieuw instellen**. Het wachtwoord op het lokale beheerdersaccount die u opgeeft wanneer u niet zeker weet of u bent vergeten, de referenties Hiermee stelt u deze stap bij probleemoplossing.

    Eerst de gebruikersnaam en een nieuw wachtwoord opgeven door te klikken voor het toewijzen van referenties op de `$cred` variabele als volgt:

    ```powershell
    $cred=Get-Credential
    ```

    De referenties op uw VM nu bijwerken. Het volgende voorbeeld bijgewerkt in een VM met de naam van de referenties `myVM` in de `WestUS` locatie en klik in de resourcegroep met de naam `myResourceGroup`:

    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location WestUS -Name "myVMAccessExtension" `
        -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password
    ```

4. **Start uw VM opnieuw**. Deze stap bij probleemoplossing kan de VM zelf heeft onderliggende problemen worden gecorrigeerd.

    Het volgende voorbeeld opnieuw is opgestart de VM met de naam `myVM` in de resourcegroep naam `myResourceGroup`:

    ```powershell
    Restart-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
    ```

5. **Implementeer deze opnieuw uw VM**. Deze stap bij probleemoplossing redeploys uw VM naar een andere host binnen Azure elk onderliggende platform- of netwerkproblemen worden gecorrigeerd.

    Het volgende voorbeeld de VM met de naam redeploys `myVM` in de `WestUS` locatie en klik in de resourcegroep met de naam `myResourceGroup`:

    ```powershell
    Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

Als u nog steeds problemen RDP Doe, u kunt [openen van een verzoek voor ondersteuning](https://azure.microsoft.com/support/options/) of Lees [meer gedetailleerde RDP voor probleemoplossing concepten en stappen](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-vms-created-using-the-classic-deployment-model"></a>Problemen met VMs die zijn gemaakt met behulp van het model Klassiek-implementatie

Probeer opnieuw verbinding maken met de VM na elke stap bij probleemoplossing.

1. **Opnieuw instellen van uw RDP-verbinding**. De configuratie van de RDP opnieuw instellen wanneer externe verbindingen zijn uitgeschakeld of Windows Firewall regels RDP, bijvoorbeeld blokkeren in deze stap bij probleemoplossing.

    Selecteer uw VM in de portal van Azure. Klik op de **... Meer** naar de knop en klik op **Externe toegang opnieuw instellen**:

    ![Beginwaarden van de RDP-configuratie in de portal van Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-reset-rdp.png)

2.  **Controleer of Cloud Services-eindpunten**. Deze stap wordt gecontroleerd of dat er eindpunten in de Cloud Services toe te staan RDP-verkeer is toegestaan. De standaardpoort voor RDP is TCP-poort 3389. Een regel toe te staan RDP-verkeer mogelijk niet automatisch worden gemaakt wanneer u uw VM maakt.

    Selecteer uw VM in de portal van Azure. Klik op de knop **eindpunten** om weer te geven van de eindpunten die momenteel zijn geconfigureerd voor uw VM. Controleer of de eindpunten waarmee RDP-verkeer op TCP-poort 3389 bestaan.
    
    Het volgende voorbeeld ziet geldige eindpunten waarmee RDP-verkeer:

    ![Controleer of de eindpunten Cloud Services in de portal van Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-verify-cloud-services-endpoints.png)

    Als u nog geen een eindpunt waarmee RDP-verkeer, [een Cloud Services-eindpunt te maken](virtual-machines-windows-classic-setup-endpoints.md). TCP privé poort 3389 toestaan.

3. **Controleren VM opstarten diagnostische gegevens**. Deze stap bij probleemoplossing reviseert het VM console-Logboeken om te bepalen als de VM is worden gebruikt voor het melden van een probleem. Niet alle VMs hebben opstarten diagnostische hulpprogramma's die zijn ingeschakeld, dus het is mogelijk dat deze stap bij probleemoplossing optioneel.
    
    Bepaalde stappen voor probleemoplossing buiten het bereik van dit artikel zijn, maar kunnen duiden op een grotere probleem dat dat dit gevolgen RDP connectivity heeft is. Zie voor meer informatie over het reviseren van de console-logboeken en VM schermafbeelding, [Opstarten diagnostische hulpprogramma's voor VMs](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

4. **De status van de Resource VM controleren**. Er zijn geen bekende problemen met het Azure platform die van invloed op connectiviteit met de VM zijn mogelijk wordt gecontroleerd met deze stap bij probleemoplossing.

    Selecteer uw VM in de portal van Azure. Schuif omlaag in het deelvenster instellingen naar de sectie **ondersteuning + probleemoplossing** bijna onder aan de lijst. Klik op de knop **Status van de Resource** . Een correct VM rapporten die **beschikbaar**:

    ![VM resource status in de portal van Azure controleren](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-check-resource-health.png)

5. De **gebruikersreferenties opnieuw instellen**. Het wachtwoord op het lokale beheerdersaccount die u opgeeft wanneer u niet zeker weet of de referenties bent vergeten Hiermee stelt u deze stap bij probleemoplossing.

    Selecteer uw VM in de portal van Azure. Schuif omlaag in het deelvenster instellingen naar de sectie **ondersteuning + probleemoplossing** bijna onder aan de lijst. Klik op de knop **wachtwoord opnieuw instellen** . Typ uw gebruikersnaam en een nieuw wachtwoord. Klik tot slot op de knop **Opslaan** :

    ![De gebruikersreferenties in de portal van Azure opnieuw instellen](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-reset-password.png)

6. **Start uw VM opnieuw**. Deze stap bij probleemoplossing kan de VM zelf heeft onderliggende problemen worden gecorrigeerd.

    Uw VM in de portal van Azure en klik op het tabblad **Overzicht** . Klik op de knop **opnieuw starten** :

    ![Start de VM opnieuw in de portal van Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-restart-vm.png)
    
Als u nog steeds problemen RDP Doe, u kunt [openen van een verzoek voor ondersteuning](https://azure.microsoft.com/support/options/) of Lees [meer gedetailleerde RDP voor probleemoplossing concepten en stappen](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-specific-rdp-errors"></a>Specifieke RDP oplossen
U kunt een specifiek foutbericht kan optreden als u verbinding maakt met uw VM via RDP. Hierna ziet u de meest voorkomende foutberichten:

- [De externe sessie is beëindigd omdat er geen beschikbaar op te geven van een licentie externe bureaublad licentieservers](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdplicense).
- [Extern bureaublad "naam" van de computer niet vinden](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpname).
- [Verificatie fout opgetreden. Local Security Authority niet bereikbaar](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpauth).
- [Windows-beveiliging-fout: uw referenties niet werken](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#wincred).
- Op [deze computer kan geen verbinding maken met de externe computer](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpconnect).


## <a name="additional-resources"></a>Aanvullende informatie
Als geen van deze fouten opgetreden en u nog steeds geen verbinding met de VM via Extern bureaublad, leest u de gedetailleerde [problemen oplossen met extern bureaublad](virtual-machines-windows-detailed-troubleshoot-rdp.md).

- [Azure diagnostics-pakket van IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)
- Stappen voor probleemoplossing bij het openen van toepassingen op een VM, raadpleegt u [problemen oplossen met de toegang tot een toepassing wordt uitgevoerd op een VM Azure](virtual-machines-linux-troubleshoot-app-connection.md).
- Als u hebt met het gebruik van Secure Shell (SSH problemen) verbinding maken met een VM Linux in Azure wordt aangegeven, raadpleegt u [problemen met SSH verbindingen met een VM Linux in Azure wordt aangegeven](virtual-machines-linux-troubleshoot-ssh-connection.md).
