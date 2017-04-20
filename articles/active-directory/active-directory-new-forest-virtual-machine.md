<properties
    pageTitle="Een Active Directory-bos installeren op een Azure virtuele netwerk | Microsoft Azure"
    description="Een zelfstudie waarin wordt uitgelegd hoe u een nieuwe Active Directory-bos maakt op een virtuele machine (VM) in een netwerk met een Azure Virtual."
    services="active-directory, virtual-network"
    keywords="Active directory virtuele machine, active directory-bos installeren, azure active directory-video 's "
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    tags=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="install-a-new-active-directory-forest-on-an-azure-virtual-network"></a>Een nieuwe Active Directory-bos installeren op een Azure virtuele netwerk

Dit onderwerp wordt uitgelegd hoe u een nieuwe Windows Server Active Directory-omgeving maakt op een Azure virtuele netwerk op een virtuele machine (VM) op een [Azure virtuele netwerk](../virtual-network/virtual-networks-overview.md). In dit geval worden het Azure virtuele netwerk is niet verbonden met een on-premises netwerk.

Zijn situaties waarin u ook geïnteresseerd in deze Verwante onderwerpen:

- Voor een video waarin deze stappen wordt weergegeven, Zie [hoe u een nieuwe Active Directory-bos in een netwerk met een Azure virtuele installeren](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
- U kunt desgewenst [een VPN-verbinding voor de site-naar-site configureren](../vpn-gateway/vpn-gateway-site-to-site-create.md) en installeer vervolgens een nieuw bos of een on-premises implementatie bos met een Azure virtuele netwerk uit te breiden. Zie [een Active Directory replicadomeincontroller in een Azure Virtual Network installeren](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)voor deze stappen.
-  Zie [richtlijnen voor het implementeren van Windows Server Active Directory op Azure virtuele Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx)voor algemene instructies over het installeren van Active Directory Domain Services (AD DS) in een netwerk met een Azure virtual.

## <a name="scenario-diagram"></a>Scenario-Diagram

In dit scenario moeten externe gebruikers toegang tot toepassingen die worden uitgevoerd op servers domein behoren. De VMs die de toepassingsservers worden uitgevoerd en de VMs waarop domeincontrollers zijn geïnstalleerd in hun eigen cloudservice in een netwerk met Azure virtuele geïnstalleerd. Deze zijn ook opgenomen binnen een beschikbaarheid instellen voor verbeterde fouttolerantie.

![Active Directory-bos op een virtuele machines in Azure Virtual Network ][1] 7
## <a name="how-does-this-differ-from-on-premises"></a>Hoe verschilt dit vanuit on-premises?

Er is het verschil tussen een domeincontroller installeren op Azure versus on-premises implementatie. De belangrijkste verschillen worden in de volgende tabel vermeld.

Voor het configureren van...  | On-premises implementatie  | Azure virtuele netwerk
------------- | -------------  | ------------
**IP-adres van de domeincontroller**  | Statische IP-adres van het netwerkadapter-eigenschappen toewijzen   | Voer de cmdlet Set-AzureStaticVNetIP als een statische IP-adres wilt toewijzen
**DNS-client resolvercache**  | Voorkeur en alternatieve DNS-server-mailadres in het netwerk adaptereigenschappen van leden van het domein instellen   | DNS-serveradres instellen voor de netwerkeigenschappen van het virtuele
**Opslag van Active Directory-database**  | U kunt desgewenst de standaardopslaglocatie van C:\ wijzigen  | U moet standaardopslaglocatie uit C:\ wijzigen



## <a name="create-an-azure-virtual-network"></a>Een Azure virtuele netwerk maken

1. Meld u aan bij de portal van Azure klassieke.
2. Maak een virtueel netwerk. Klik op **netwerken** > **maken een virtueel netwerk**. Gebruik de waarden in de volgende tabel om de wizard te voltooien.

    Klik op deze pagina van de wizard...  | Geef de volgende waarden
    ------------- | -------------
    **Virtual Network Details**  | <p>Name: Typ een naam voor uw virtuele netwerk</p><p>Regio: Kies de dichtstbijzijnde regio</p>
    **DNS- en VPN**  | <p>Laat de DNS-server leeg</p><p>Een VPN-optie niet selecteert</p>
    **Virtuele netwerk adres spaties**  | <p>Subnetnaam: Typ een naam voor uw subnet</p><p>Begin IP: <b>10.0.0.0</b></p><p>CIDR: <b>/24 (256)</b></p>



## <a name="create-vms-to-run-the-domain-controller-and-dns-server-roles"></a>VMs om uit te voeren van de domeincontroller en DNS-serverrollen maken

De volgende stappen voor het maken van VMs als host voor de rol van de domeincontroller naar wens. U moet ten minste twee virtuele DCs bieden fouttolerantie en redundantie implementeren. Als het Azure virtuele netwerk ten minste twee DCs dat op dezelfde manier worden geconfigureerd bevat (dat wil zeggen ze beide GCs, uitvoeren DNS server zijn en geen van beide bevat een FSMO-functie, enzovoort) plaats het VMs die die in een beschikbaarheid instellen voor verbeterde fouttolerantie DCs uitgevoerd.

Zie [Azure PowerShell gebruiken om te maken en vooraf configureren op basis van Windows virtuele Machines](../virtual-machines/virtual-machines-windows-classic-create-powershell.md)de VMs maken met behulp van Windows PowerShell in plaats van de gebruikersinterface.

1. Klik in de klassieke-portal op **Nieuw** > **berekenen** > **VM** > **Uit galerie**. Gebruik de volgende waarden om de wizard te voltooien. Accepteer de standaardwaarde voor een instelling tenzij een andere waarde wordt voorgesteld of vereist.

    Klik op deze pagina van de wizard...  | Geef de volgende waarden
    ------------- | -------------
    **Een afbeelding kiezen**  | Windows Server 2012 R2 Datacenter
    **VM configuratie**  | <p>VM Name: Typ een naam Eén etiket (zoals AzureDC1).</p><p>Nieuwe gebruikersnaam: Typ de naam van een gebruiker. Deze gebruiker is lid zijn van de lokale beheerdersgroep op de VM. Moet u deze naam voor de eerste keer aanmelden bij de VM. De ingebouwde account beheerder werkt niet.</p><p>Nieuwe wachtwoord/bevestigen: Typ een wachtwoord</p>
    **VM configuratie**  | <p>Cloudservice: Kiest u <b>een nieuwe cloudservice maken</b> voor de eerste VM en selecteer de naam van die dezelfde cloud-service wanneer u meer VMs die als host de rol van de domeincontroller fungeert maakt.</p><p>Cloud Service DNS-naam: Geef een globaal unieke naam</p><p>Land/affiniteit groep/virtuele netwerk: Geef de naam virtueel netwerk (zoals WestUSVNet).</p><p>Opslag-Account: Kies <b>een account automatisch gegenereerde opslag gebruiken</b> voor de eerste VM en selecteer vervolgens die dezelfde opslagaccountnaam wanneer u meer VMs die als host de rol van de domeincontroller fungeert maakt.</p><p>Beschikbaarheid van de Set: Kies <b>een set beschikbaarheid maken</b>.</p><p>Beschikbaarheid Stel naam: Typ een naam voor de beschikbaarheid instellen wanneer u de eerste VM maken en selecteer vervolgens dat dezelfde naam als u meer VMs maakt.</p>
    **VM configuratie**  | <p>Selecteer <b>de VM-Agent installeren</b> en eventuele andere extensies die u nodig hebt.</p>
2. Een schijf hebt toegevoegd aan elke VM die de functie van de domeincontroller server wordt uitgevoerd. De extra schijf nodig is voor de opslag van de AD-database, logboeken en SYSVOL. Een grootte voor de schijf (zoals 10 GB) aan te geven en laat de **Host Cache voorkeur** ingesteld op **geen**. Zie voor de stappen voor [het toevoegen van een schijf gegevens aan een virtuele Windows-computer](../virtual-machines/virtual-machines-windows-classic-attach-disk.md).
3. Nadat u zich eerst hebt aangemeld bij de VM, opent u **Serverbeheer** > **Bestands- en opslagservices** een volume op deze schijf met NTFS maken.
4. Een statische IP-adres reserveren voor VMs die de domeincontroller-functie wordt uitgevoerd. Als u wilt reserveren een statisch IP-adres, downloadt u het installatieprogramma van Microsoft Web Platform en [Azure PowerShell installeren](../powershell-install-configure.md) en voert u de cmdlet Set-AzureStaticVNetIP. Bijvoorbeeld:

    ' Get-AzureVM - servicenaam AzureDC1-naam AzureDC1 | Set-AzureStaticVNetIP - IP-adres adres 10.0.0.4 | Update-AzureVM

Zie voor meer informatie over het instellen van een statische IP-adres [configureren een statische interne IP-adres van een VM](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-windows-server-active-directory"></a>Installeer Windows Server Active Directory

Gebruik dezelfde routine voor het [installeren van AD DS](https://technet.microsoft.com/library/jj574166.aspx) on-premises implementatie te gebruiken (dat wil zeggen, kunt u de gebruikersinterface, een standaardsjabloon of Windows PowerShell). U moet beheerdersreferenties om te kunnen installeren van een nieuw bos opgeven. Wanneer u de locatie voor de Active Directory-database, logboeken en SYSVOL, wijzigt u de standaardopslaglocatie van het besturingssysteem harde schijf in de aanvullende gegevensschijf die u hebt gekoppeld aan de VM.

Nadat de installatie van de domeincontroller is voltooid, opnieuw verbinding maakt met de VM en meld u aan bij de domeincontroller. Onthoud dat domeinreferenties opgeven.

## <a name="reset-the-dns-server-for-the-azure-virtual-network"></a>Beginwaarden van de DNS-server voor het Azure virtuele netwerk

1. Opnieuw instellen als de DNS-doorstuurservers-instelling op de nieuwe domeincontroller/DNS-server.
  1. In Serverbeheer, klik op **Extra** > **DNS**.
  2. **DNS-beheer**, met de rechtermuisknop op de naam van de DNS-server en klik op **Eigenschappen**.
  3. Klik op het tabblad **doorstuurservers** het IP-adres van de doorstuurservers op en klik op **bewerken**.  Selecteer het IP-adres en klikt u op **verwijderen**.
  4. Klik op **OK** om de te sluiten en **Ok** opnieuw in te sluiten van de DNS-server-eigenschappen.
2. Werk de DNS-server-instelling voor het virtuele netwerk.
  1. Klik op **Virtuele netwerken** > Dubbelklik op het virtuele netwerk dat u hebt gemaakt > **configureren** > **DNS-servers**, typ de naam en de DIP van een van de VMs die wordt uitgevoerd van de functie van de domeincontroller/DNS-server en klik op **Opslaan**.
  2. Selecteer de VM en op **opnieuw** activeren de VM DNS-resolvercache om instellingen te configureren met het IP-adres van de nieuwe DNS-server.


## <a name="create-vms-for-domain-members"></a>VMs maken voor leden van het domein

1. Herhaal de volgende stappen uit als u wilt maken van VMs om uit te voeren als toepassingsservers. Accepteer de standaardwaarde voor een instelling tenzij een andere waarde wordt voorgesteld of vereist.

    Klik op deze pagina van de wizard...  | Geef de volgende waarden
    ------------- | -------------
    **Een afbeelding kiezen**  | Windows Server 2012 R2 Datacenter
    **VM configuratie**  | <p>VM Name: Typ een naam Eén etiket (zoals AppServer1).</p><p>Nieuwe gebruikersnaam: Typ de naam van een gebruiker. Deze gebruiker is lid zijn van de lokale beheerdersgroep op de VM. Moet u deze naam voor de eerste keer aanmelden bij de VM. De ingebouwde account beheerder werkt niet.</p><p>Nieuwe wachtwoord/bevestigen: Typ een wachtwoord</p>
    **VM configuratie**  | <p>Cloudservice: Kiest u **een nieuwe cloudservice maken** voor de eerste VM en selecteer de naam van die dezelfde cloud-service wanneer u meer VMs die als host de toepassing fungeert maakt.</p><p>Cloud Service DNS-naam: Geef een globaal unieke naam</p><p>Land/affiniteit groep/virtuele netwerk: Geef de naam virtueel netwerk (zoals WestUSVNet).</p><p>Opslag-Account: Kies **een account automatisch gegenereerde opslag gebruiken** voor de eerste VM en selecteer vervolgens die dezelfde opslagaccountnaam wanneer u meer VMs die als host de toepassing fungeert maakt.</p><p>Beschikbaarheid van de Set: Kies **een set beschikbaarheid maken**.</p><p>Beschikbaarheid Stel naam: Typ een naam voor de beschikbaarheid instellen wanneer u de eerste VM maken en selecteer vervolgens dat dezelfde naam als u meer VMs maakt.</p>
    **VM configuratie**  | <p>Selecteer <b>de VM-Agent installeren</b> en eventuele andere extensies die u nodig hebt.</p>
2. Nadat u elke VM is ingericht, aanmelden en deze toevoegen aan het domein. Klik op **Lokale Server**in **Serverbeheer** > **werkgroep** > **wijzigen...** en selecteer vervolgens **domein** en typ de naam van uw on-premises domein. Uw referenties van de domeingebruiker van een en start de VM om te voltooien van de join domein.

Zie [Azure PowerShell gebruiken om te maken en vooraf configureren op basis van Windows virtuele Machines](../virtual-machines/virtual-machines-windows-classic-create-powershell.md)de VMs maken met behulp van Windows PowerShell in plaats van de gebruikersinterface.

Zie voor meer informatie over het gebruik van Windows PowerShell [Aan de slag met Azure-Cmdlets](https://msdn.microsoft.com/library/azure/jj554332.aspx) en [Azure Cmdlet verwijzing](https://msdn.microsoft.com/library/azure/jj554330.aspx).


## <a name="see-also"></a>Zie ook

-  [Hoe u een nieuwe Active Directory-bos installeert op een Azure virtuele netwerk](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
-  [Richtlijnen voor het implementeren van Windows Server Active Directory op Azure virtuele Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx)

-  [Een VPN-verbinding voor de Site-naar-Site configureren](../vpn-gateway/vpn-gateway-site-to-site-create.md)
-  [Een Active Directory replicadomeincontroller installeren in een Azure virtuele netwerk](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)
-  [Microsoft Azure IT Pro IaaS: (01) VM over grondbeginselen](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure IT Pro IaaS: (05) maken van virtuele netwerken en Cross-Premises-connectiviteit](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Overzicht van Virtual Network](../virtual-network/virtual-networks-overview.md)
-  [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md)
-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Azure-Cmdlet-verwijzing](https://msdn.microsoft.com/library/azure/jj554330.aspx)
-  [Azure VM statisch IP-adres instellen](http://windowsitpro.com/windows-azure/set-azure-vm-static-ip-address)
-  [Het statische IP toewijzen aan Azure VM](http://www.bhargavs.com/index.php/2014/03/13/how-to-assign-static-ip-to-azure-vm/)
-  [Een nieuwe Active Directory-bos installeren](https://technet.microsoft.com/library/jj574166.aspx)
-  [Inleiding tot Active Directory Domain Services (AD DS) Virtualization (niveau 100)](https://technet.microsoft.com/library/hh831734.aspx)

<!--Image references-->
[1]: ./media/active-directory-new-forest-virtual-machine/AD_Forest.png
