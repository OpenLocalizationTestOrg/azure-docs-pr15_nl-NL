<properties
    pageTitle="Een installatiefout met een replicadomeincontroller voor Active Directory in Azure | Microsoft Azure"
    description="Een zelfstudie waarin wordt uitgelegd hoe u een domeincontroller installeren vanaf een lokale Active Directory-bos op een Azure virtuele machine."
    services="virtual-network"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="virtual-network"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="curtand"/>


# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Een replicadomeincontroller voor Active Directory in een netwerk met Azure virtuele installeren

Dit onderwerp wordt uitgelegd hoe u installeert controller (ook wel bekend als replica DCs) voor een on-premises Active Directory-domein op Azure virtuele machines (VMs) in een Azure virtuele netwerk.

Zijn situaties waarin u ook geïnteresseerd in deze Verwante onderwerpen:

-  Desgewenst kunt u een nieuwe Active Directory-bos installeren op een Azure virtuele netwerk. Zie [een nieuwe Active Directory-bos in een netwerk met een Azure virtuele installeren](../active-directory/active-directory-new-forest-virtual-machine.md)voor deze stappen.
-  Zie [richtlijnen voor het implementeren van Windows Server Active Directory op Azure virtuele Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx)voor conceptuele instructies over het installeren van Active Directory Domain Services (AD DS) in een netwerk met een Azure virtual.


## <a name="scenario-diagram"></a>Scenario-diagram

In dit scenario moeten externe gebruikers toegang tot toepassingen die worden uitgevoerd op servers domein behoren. De VMs die de toepassingsservers worden uitgevoerd en de DCs replica in een netwerk met Azure virtual zijn geïnstalleerd. Het virtuele netwerk kan worden verbonden met het lokale netwerk door een [site-naar-site VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) -verbinding, zoals wordt weergegeven in het volgende diagram of kunt u [ExpressRoute](../expressroute/expressroute-locations-providers.md) voor een snellere verbinding.

De toepassingsservers en de DCs zijn geïmplementeerd in afzonderlijke cloudservices distributie van berekeningscluster verwerking en [beschikbaarheid sets](../virtual-machines/virtual-machines-windows-manage-availability.md) voor verbeterde fouttolerantie.
De DCs repliceren met elkaar en met on-premises implementatie DCs met behulp van Active Directory-replicatie. Geen extra synchronisatie nodig zijn.

![Diagram af Active Directory replicadomeincontroller een Azure vnet][1]

## <a name="create-an-active-directory-site-for-the-azure-virtual-network"></a>Een Active Directory-site voor het Azure virtuele netwerk maken

Dit is een goed idee om een site maken in Active Directory die staat voor het netwerk gebied dat overeenkomt met het virtuele netwerk. Die helpt verificatie, herhaling en andere bewerkingen van de locatie domeincontroller optimaliseren. De volgende stappen wordt uitgelegd hoe u een site maken en voor meer achtergrondinformatie, raadpleegt [u een nieuwe Site toevoegt](https://technet.microsoft.com/library/cc781496.aspx).

1. Open Active Directory-Sites en Services: **Serverbeheer** > **Hulpmiddelen voor** > **Active Directory-Sites en Services**.
2. Een site voor de regio waar u een Azure virtuele netwerk gemaakt maken: klik op **Sites** > **actie** > **nieuwe site** > Typ de naam van de nieuwe site, zoals Azure ons West > selecteert u een koppeling naar site > **OK**.
3. Een subnet maken en koppelen aan de nieuwe site: Dubbelklik op **Sites** > met de rechtermuisknop op **subnetten** > **Nieuw subnet** > Typ het bereik van de IP-adres van het virtuele netwerk (zoals 10.1.0.0/16 in het diagram scenario) > selecteert u de nieuwe Azure site > **OK**.

## <a name="create-an-azure-virtual-network"></a>Een Azure virtuele netwerk maken

1. Klik op **Nieuw**in de [portal van Azure klassieke](https://manage.windowsazure.com) > **Netwerkservices** > **Virtual Network** > **Aangepaste maken** en gebruik de volgende waarden om de wizard te voltooien.

    Klik op deze pagina van de wizard...  | Geef de volgende waarden
    ------------- | -------------
    **Virtual Network Details**  | <p>Name: Typ een naam voor het virtuele netwerk, zoals WestUSVNet.</p><p>Regio: Kies de dichtstbijzijnde regio.</p>
    **DNS- en VPN-verbinding**  | <p>DNS-Servers: Geef de naam en het IP-adres van een of meer lokale DNS-servers.</p><p>Connectiviteit: Selecteer **een VPN-verbinding voor de site-naar-site configureren**.</p><p>Lokale netwerk: een nieuwe lokale netwerk kiezen.</p><p>Als u ExpressRoute in plaats van een VPN-verbinding gebruikt, raadpleegt u [een verbinding ExpressRoute via een Exchange-Provider configureren](../expressroute/expressroute-locations-providers.md).</p>
    **Site-naar-site-connectiviteit**  | <p>Name: Typ een naam voor de on-premises netwerk.</p><p>VPN apparaat IP-adres: het openbare IP-adres van het apparaat dat verbinding met het virtuele netwerk maken opgeven. Het apparaat VPN kan niet worden gevonden achter een NAT bevindt.</p><p>Adres: Geef de adresbereiken voor uw on-premises netwerk (zoals 192.168.0.0/16 in het diagram scenario).</p>
    **Virtuele netwerk adres spaties**  | <p>Adresruimte: Geef het IP-adresbereiken voor VMs die u wilt uitvoeren in het Azure virtuele netwerk (zoals 10.1.0.0/16 in het diagram scenario). In dit adresbereik elkaar niet overlappen met de adresbereiken van de on-premises netwerk.</p><p>Subnetten: Geef een naam en adres voor een subnet voor de toepassingsservers (zoals Frontend, 10.1.1.0/24) en voor de (zoals Backend, 10.1.2.0/24).</p><p>Klik op **gateway subnet toevoegen**.</p>

2. Vervolgens moet u de gateway virtueel netwerk om te maken van een beveiligde site-naar-site VPN-verbinding configureren. Zie [een virtuele netwerkgateway configureren](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) voor de instructies.
3. De site-naar-site VPN-verbinding maken tussen het nieuwe virtuele netwerk en een on-premises implementatie VPN-apparaat. Zie [een virtuele netwerkgateway configureren](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) voor de instructies.


## <a name="create-azure-vms-for-the-dc-roles"></a>Azure VMs maken voor de domeincontroller rollen

De volgende stappen voor het maken van VMs als host voor de rol van de domeincontroller naar wens. U moet ten minste twee virtuele DCs bieden fouttolerantie en redundantie implementeren. Als het Azure virtuele netwerk ten minste twee DCs dat op dezelfde manier worden geconfigureerd bevat (dat wil zeggen ze beide GCs, uitvoeren DNS server zijn en geen van beide bevat een FSMO-functie, enzovoort) plaats het VMs die die in een beschikbaarheid instellen voor verbeterde fouttolerantie DCs uitgevoerd.
Zie [Azure PowerShell gebruiken om te maken en vooraf configureren op basis van Windows virtuele Machines](../virtual-machines/virtual-machines-windows-classic-create-powershell.md)de VMs maken met behulp van Windows PowerShell in plaats van de gebruikersinterface.

1. Klik op **Nieuw**in de [portal van Azure klassieke](https://manage.windowsazure.com) > **berekenen** > **VM** > **Uit galerie**. Gebruik de volgende waarden om de wizard te voltooien. Accepteer de standaardwaarde voor een instelling tenzij een andere waarde wordt voorgesteld of vereist.

    Klik op deze pagina van de wizard...  | Geef de volgende waarden
    ------------- | -------------
    **Een afbeelding kiezen**  | Windows Server 2012 R2 Datacenter
    **VM configuratie**  | <p>VM Name: Typ een naam Eén etiket (zoals AzureDC1).</p><p>Nieuwe gebruikersnaam: Typ de naam van een gebruiker. Deze gebruiker is lid zijn van de lokale beheerdersgroep op de VM. Moet u deze naam voor de eerste keer aanmelden bij de VM. De ingebouwde account beheerder werkt niet.</p><p>Nieuwe wachtwoord/bevestigen: Typ een wachtwoord</p>
    **VM configuratie**  | <p>Cloudservice: Kiest u <b>een nieuwe cloudservice maken</b> voor de eerste VM en selecteer de naam van die dezelfde cloud-service wanneer u meer VMs die als host de rol van de domeincontroller fungeert maakt.</p><p>Cloud Service DNS-naam: Geef een globaal unieke naam</p><p>Land/affiniteit groep/virtuele netwerk: Geef de naam virtueel netwerk (zoals WestUSVNet).</p><p>Opslag-Account: Kies <b>een account automatisch gegenereerde opslag gebruiken</b> voor de eerste VM en selecteer vervolgens die dezelfde opslagaccountnaam wanneer u meer VMs die als host de rol van de domeincontroller fungeert maakt.</p><p>Beschikbaarheid van de Set: Kies <b>een set beschikbaarheid maken</b>.</p><p>Beschikbaarheid Stel naam: Typ een naam voor de beschikbaarheid instellen wanneer u de eerste VM maken en selecteer vervolgens dat dezelfde naam als u meer VMs maakt.</p>
    **VM configuratie**  | <p>Selecteer <b>de VM-Agent installeren</b> en eventuele andere extensies die u nodig hebt.</p>
2. Een schijf hebt toegevoegd aan elke VM die de functie van de domeincontroller server wordt uitgevoerd. De extra schijf nodig is voor de opslag van de AD-database, logboeken en SYSVOL. Een grootte voor de schijf (zoals 10 GB) aan te geven en laat de **Host Cache voorkeur** ingesteld op **geen**. Zie voor de stappen voor [het toevoegen van een schijf gegevens aan een virtuele Windows-computer](../virtual-machines/virtual-machines-windows-classic-attach-disk.md).
3. Nadat u zich eerst hebt aangemeld bij de VM, opent u **Serverbeheer** > **Bestands- en opslagservices** een volume op deze schijf met NTFS maken.
4. Een statische IP-adres reserveren voor VMs die de domeincontroller-functie wordt uitgevoerd. Als u wilt reserveren een statische IP-adres, downloadt u het installatieprogramma van Microsoft Web Platform en [Azure PowerShell installeren](../powershell-install-configure.md) en voert u de cmdlet Set-AzureStaticVNetIP. Bijvoorbeeld:

    ' Get-AzureVM - servicenaam AzureDC1-naam AzureDC1 | Set-AzureStaticVNetIP - IP-adres adres 10.0.0.4 | Update-AzureVM

Zie voor meer informatie over het instellen van een statische IP-adres [configureren een statische interne IP-adres van een VM](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>AD DS op Azure VMs installeren

Meld u aan bij een VM en controleer of u connectivity via de website naar VPN of ExpressRoute verbinding naar bronnen op uw on-premises netwerk. Installeer AD DS op de VMs Azure. Hier kunt u dezelfde stappen waarmee u kunt een extra domeincontroller installeren op uw on-premises netwerk (UI, Windows PowerShell of een antwoordbestand). Als u AD DS hebt geïnstalleerd, zorg er dan voor dat u het nieuwe volume voor de locatie van de AD-database, logboeken en SYSVOL opgeven. Als u nodig hebt voor een geheugensteun AD DS-installatie, raadpleegt u [Installeren Active Directory Domain Services (niveau 100)](https://technet.microsoft.com/library/hh472162.aspx) of het [installeren van een Windows Server 2012 replicadomeincontroller in een bestaand domein (niveau 200)](https://technet.microsoft.com/library/jj574134.aspx).

## <a name="reconfigure-dns-server-for-the-virtual-network"></a>DNS-server voor het virtuele netwerk opnieuw configureren

1. In de [klassieke Azure-portal](https://manage.windowsazure.com), klik op de naam van het virtuele netwerk en klik vervolgens op het tabblad **configureren** om opnieuw te [configureren, de DNS-server IP-adressen voor uw netwerk op virtuele](../virtual-network/virtual-networks-manage-dns-in-vnet.md) gebruik van de statische IP-adressen die zijn toegewezen aan de replica DCs in plaats van het IP-adressen van een on-premises implementatie DNS-servers.

2. Om ervoor te zorgen dat alle replica domeincontroller VMs op het virtuele netwerk met gebruik van de DNS-servers op het virtuele netwerk zijn geconfigureerd, klikt u op **virtuele Machines**, klikt u op de statuskolom voor elke VM en klik op **opnieuw starten**. Wacht totdat de VM staat **uitgevoerd ziet** voordat u probeert u zich aanmeldt bij deze.

## <a name="create-vms-for-application-servers"></a>VMs voor toepassingsservers maken

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

## <a name="additional-resources"></a>Aanvullende informatie

-  [Richtlijnen voor het implementeren van Windows Server Active Directory op Azure virtuele Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx)
-  [Het uploaden van bestaande on-premises implementatie Hyper-V domeincontrollers naar Azure met behulp van Azure PowerShell](http://support.microsoft.com/kb/2904015)
-  [Een nieuwe Active Directory-bos installeren op een Azure virtuele netwerk](../active-directory/active-directory-new-forest-virtual-machine.md)
-  [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
-  [Microsoft Azure IT Pro IaaS: (01) VM over grondbeginselen](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure IT Pro IaaS: (05) maken van virtuele netwerken en Cross-Premises-connectiviteit](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Cmdlets voor gebruikersbeheer van Azure](https://msdn.microsoft.com/library/azure/jj152841)

<!--Image references-->
[1]: ./media/active-directory-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png
