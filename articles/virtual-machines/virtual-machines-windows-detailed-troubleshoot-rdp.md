<properties
    pageTitle="Gedetailleerde probleemoplossing extern bureaublad | Microsoft Azure"
    description="Gedetailleerde voor probleemoplossing stappen voor het externe bureaublad fouten waar u kunt niet naar een Windows virtuele machines in Azure controleren"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"
    keywords="kan geen verbinding met extern bureaublad, problemen met extern bureaublad, extern bureaublad kan geen verbinding maken, extern bureaublad fouten, probleemoplossing extern bureaublad, extern bureaublad problemen
"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="detailed-troubleshooting-steps-for-remote-desktop-connection-issues-to-windows-vms-in-azure"></a>Gedetailleerde stappen voor probleemoplossing verbinding met extern bureaublad problemen naar Windows VMs in Azure wordt aangegeven

In dit artikel bevat gedetailleerde stappen voor probleemoplossing automatisch opsporen en oplossen complexe extern bureaublad voor op basis van Windows Azure virtuele machines.

> [AZURE.IMPORTANT] Als u wilt verwijderen in de meer veelvoorkomende fouten in extern bureaublad, moet Lees [het artikel eenvoudige voor extern bureaublad](virtual-machines-windows-troubleshoot-rdp-connection.md) voordat u verdergaat.

Een extern bureaublad foutbericht wordt weergegeven die op een van de specifieke foutberichten beschreven in [de eenvoudige gids voor probleemoplossing in extern bureaublad](virtual-machines-windows-troubleshoot-rdp-connection.md)niet lijken kunnen optreden. Volg deze stappen om te bepalen waarom de client Remote Desktop (RDP) kan geen verbinding maken met de RDP-service op de VM Azure.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Als u meer hulp op een willekeurige plaats in dit artikel nodig hebt, kunt u contact opnemen met de Azure experts op [het MSDN-Azure en de stapel overloop-forums](https://azure.microsoft.com/support/forums/). U kunt ook kunt u ook een incident Azure ondersteuning indienen. Ga naar de [site van Azure-ondersteuning](https://azure.microsoft.com/support/options/) en **Ondersteuning krijgen**op. Lees de [Veelgestelde vragen over het Microsoft Azure ondersteuning](https://azure.microsoft.com/support/faq/)voor informatie over het gebruik van Azure-ondersteuning.


## <a name="components-of-a-remote-desktop-connection"></a>Onderdelen van een verbinding met extern bureaublad

De volgende onderdelen zijn betrokken bij een RDP-verbinding:

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_0.png)

Voordat u verder gaat, is het mogelijk handiger geestelijk bekijken wat heeft gewijzigd sinds de laatste geslaagde extern bureaublad-verbinding voor VM. Bijvoorbeeld:

- Het openbare IP-adres van de VM of met de cloudservice met de VM (ook wel het virtuele IP-adres [VIP](https://en.wikipedia.org/wiki/Virtual_IP_address)genoemd) is gewijzigd. De fout RDP kan zijn dat uw DNS-client-cache nog steeds het *oude IP-adres* geregistreerd voor de DNS-naam heeft. Uw DNS-client-cache leegmaken en probeert u de VM opnieuw verbinding te maken. Of probeer verbinding te maken met de nieuwe VIP rechtstreeks.
- U gebruikt een derde partij-toepassing voor het beheren van uw verbindingen met extern bureaublad in plaats van de verbinding die zijn gegenereerd door de Azure-portal. Controleer of de configuratie van de toepassing bevat de juiste TCP-poort voor het verkeer extern bureaublad. U kunt deze poort voor een klassiek virtuele machine in de [portal van Azure](https://portal.azure.com)controleren door te klikken op van de VM-instellingen > eindpunten.


## <a name="preliminary-steps"></a>Voorbereidende stappen

Voordat u verder gaat naar de gedetailleerde probleemoplossing

- Controleer de status van de virtuele machine in de portal van Azure klassieke of de Azure portal voor eventuele problemen met de hand liggende.
- Volg de [stappen retoucheren voor veelvoorkomende fouten in RDP in de eenvoudige gids voor probleemoplossing](virtual-machines-windows-troubleshoot-rdp-connection.md#quick-troubleshooting-steps).


Probeert u opnieuw verbinding maken met de VM via Extern bureaublad na deze stappen.


## <a name="detailed-troubleshooting-steps"></a>Gedetailleerde stappen voor probleemoplossing

De client extern bureaublad mogelijk niet kunnen bereiken van de service Extern bureaublad op de VM Azure vanwege problemen bij de volgende bronnen:

- [Extern-bureaubladclient computer](#source-1-remote-desktop-client-computer)
- [Organisatie intranet edge-apparaat](#source-2-organization-intranet-edge-device)
- [Service-eindpunt cloud en toegangsbeheerlijsten (ACL)](#source-3-cloud-service-endpoint-and-acl)
- [Beveiligingsgroepen netwerk](#source-4-network-security-groups)
- [Op basis van Windows Azure VM](#source-5-windows-based-azure-vm)

## <a name="source-1-remote-desktop-client-computer"></a>Bron 1: Extern-bureaubladclient computer

Controleer of dat uw computer extern bureaublad verbinding met een ander on-premises computer met Windows kunt maken.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_1.png)

Als u niet kunt, kunt u controleren op de volgende instellingen op uw computer:

- Een lokale firewall instellen die wordt geblokkeerd door extern bureaublad-verkeer is toegestaan.
- Lokaal proxy clientsoftware waardoor extern bureaublad verbindingen hebt geïnstalleerd.
- Lokaal netwerk monitoring software waardoor extern bureaublad-verbindingen zijn geïnstalleerd.
- Andere soorten beveiligingssoftware die om verkeer te controleren of specifieke soorten verkeer waardoor extern bureaublad verbindingen toestaan/weigeren.

In al deze gevallen, tijdelijk uitschakelen van de software en probeer het verbinding maken met een lokale computer via Extern bureaublad. Als u de werkelijke oorzaak deze manier vinden kunt, kunt u werken met uw netwerkbeheerder om te corrigeren van de software-instellingen om extern bureaublad verbindingen te staan.

## <a name="source-2-organization-intranet-edge-device"></a>Bron 2: Organisatie intranet edge-apparaat

Controleer of die een computer die rechtstreeks verbonden met Internet extern bureaublad-verbindingen met uw Azure virtuele machines kunt aanbrengen.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_2.png)

Als u nog geen een computer die rechtstreeks met Internet verbonden, maken en test met een nieuwe Azure virtuele machine in een groep of cloud-service van resource. Zie [een virtuele machine maken waarop Windows in Azure wordt aangegeven wordt uitgevoerd](virtual-machines-windows-hero-tutorial.md)voor meer informatie. U kunt de virtuele machine en de resourcegroep of de cloudservice verwijderen na de test.

Als u een verbinding met extern bureaublad met een computer die rechtstreeks verbonden met Internet maken kunt, controleert u uw organisatie intranet edge-apparaat voor:

- Een interne firewall geblokkeerd door HTTPS-verbindingen met Internet.
- Een proxyserver extern bureaublad verbindingen te blokkeren.
- Detectie van een geopende of netwerk monitoring software uitvoeren op apparaten in uw netwerk rand waardoor verbindingen met extern bureaublad.

Werken met uw netwerkbeheerder om te corrigeren van de instellingen van uw organisatie intranet edge-apparaat om extern bureaublad HTTPS gebaseerde verbindingen met Internet te staan.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Bron 3: De Cloud service-eindpunt en ACL

Controleer voor VMs gemaakt met behulp van het model Klassiek implementatie, of dat een andere Azure VM die zich in dezelfde cloudservice- of virtueel netwerk extern bureaublad-verbindingen in uw VM Azure aanbrengen kunt.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_3.png)

> [AZURE.NOTE] Voor virtuele machines in resourcemanager hebt gemaakt, gaat u verder met [bron 4: netwerk beveiligingsgroepen](#source-4-network-security-groups).

Als u een andere VM in een virtuele netwerk of dezelfde cloudservice niet hebt, een maken. Volg de stappen in [een virtuele machine maken waarop Windows in Azure wordt aangegeven wordt uitgevoerd](virtual-machines-windows-hero-tutorial.md). De test virtuele machine verwijderen nadat de test is voltooid.

Als u verbinding via Extern bureaublad met een virtuele machine in een virtuele netwerk of dezelfde cloudservice maken kunt, kunt u controleren op deze instellingen:

- De eindpuntconfiguratie voor extern bureaublad-verkeer is toegestaan op het doel VM: de persoonlijke TCP-poort van het eindpunt moet overeenkomen met de TCP-poorten waarop u van de VM extern bureaublad service luistert (standaard is 3389).
- De ACL voor het eindpunt van de verkeer extern bureaublad op het doel VM: ACL's kunnen u opgeven toegestaan of binnenkomende verkeer van Internet op basis van het bron-IP-adres geweigerd. Zijn onjuist geconfigureerd ACL's kunnen voorkomen dat van binnenkomende extern bureaublad-verkeer door naar het eindpunt. Controleer uw ACL's om ervoor te zorgen dat binnenkomende verkeer van uw openbare IP-adressen van uw proxy of andere randserver is toegestaan. Zie voor meer informatie [Wat is een netwerk Access (Toegangsbeheerlijst)?](../virtual-network/virtual-networks-acl.md)

Het huidige eindpunt verwijderen om te controleren of het eindpunt de bron van het probleem, en maak een nieuwe, een willekeurige poort kiezen in het bereik 49152 – 65535 voor het poortnummer in externe. Lees [hoe u het eindpunten naar een virtuele machine instellen](virtual-machines-windows-classic-setup-endpoints.md)voor meer informatie.

## <a name="source-4-network-security-groups"></a>Bron 4: Netwerk beveiligingsgroepen

Beveiligingsgroepen netwerk toestaan toegestane binnenkomende en uitgaande verkeer meer detail kunnen beheren. U kunt regels die in beslag nemen subnetten maken en cloud services in een Azure virtuele netwerk. Controleer uw netwerk-beveiligingsgroep regels om ervoor te zorgen dat extern bureaublad verkeer van Internet is toegestaan:

- Selecteer in de portal Azure uw VM
- Klik op **alle instellingen** | **netwerk-interfaces** en selecteert u de netwerkinterface van uw.
- Klik op **alle instellingen** | **beveiligingsgroep netwerk** en selecteer de beveiligingsgroep van uw netwerk.
- Klik op **alle instellingen** | **regels voor binnenkomende** en controleer of er een regel RDP op TCP-poort 3389 toestaan.
    - Als u een regel niet hebt, klikt u op **toevoegen** als een regel wilt maken. Voer **TCP** voor het protocol en klik op **3389** voor het bereik van de poort bestemming.
    - Controleer of dat de bewerking is ingesteld op **toestaan** en klik op OK om het opslaan van uw nieuwe binnenkomende regel.


Zie voor meer informatie [Wat is een netwerk beveiliging groep (NSG)?](../virtual-network/virtual-networks-nsg.md)

## <a name="source-5-windows-based-azure-vm"></a>Bron 5: Op basis van Windows Azure VM

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_5.png)

Gebruik het [Azure IaaS (Windows) diagnostics-pakket](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864) om te controleren of de fout vanwege de Azure virtuele machine zelf. Als dit diagnostics-pakket niet kan voor het oplossen van het probleem **RDP-connectiviteit met een Azure VM (opnieuw opstarten vereist)** , volgt u de instructies in [dit artikel](virtual-machines-windows-reset-rdp.md). In dit artikel Hiermee stelt u de service Extern bureaublad op de virtuele machine opnieuw:

- Schakel de regel in 'Extern bureaublad' Windows Firewall standaard (TCP-poort 3389).
- Extern bureaublad verbindingen hebt ingeschakeld door de registerwaarde HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections 0.

Probeer opnieuw verbinding vanaf uw computer. Als u nog steeds niet kunt via Extern bureaublad te verbinden, raadpleegt u de volgende mogelijke oorzaken:

- De service Extern bureaublad wordt niet op het doel VM uitgevoerd.
- De service Extern bureaublad is niet beschikbaar op TCP-poort 3389.
- Windows Firewall of een andere lokale firewall heeft een uitgaande regel waardoor extern bureaublad-verkeer is toegestaan.
- Detectie van een geopende of netwerk software die wordt uitgevoerd op de Azure virtuele machine cmdlets voor controle is de extern bureaublad verbindingen te blokkeren.

Voor VMs gemaakt met behulp van het implementatiemodel klassieke, kunt u een externe Azure PowerShell-sessie met de Azure virtuele machine. Eerst moet u een certificaat voor de VM hostingservice cloud installeren. Ga naar [Secure externe PowerShell toegang configureren naar Azure virtuele Machines](http://gallery.technet.microsoft.com/scriptcenter/Configures-Secure-Remote-b137f2fe) en de **InstallWinRMCertAzureVM.ps1** script-bestand downloaden naar uw lokale computer.

Installeer vervolgens Azure PowerShell als u nog niet is gedaan. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

Open een opdrachtprompt Azure PowerShell en wijzigen van de huidige map naar de locatie van de **InstallWinRMCertAzureVM.ps1** script-bestand. Als u wilt een Azure PowerShell-script uitvoeren, moet u de juiste beleid instellen. De opdracht **Get-ExecutionPolicy** om te bepalen van uw huidige beleidsniveau uitvoeren. Zie [Set-ExecutionPolicy](https://technet.microsoft.com/library/hh849812.aspx)voor informatie over het instellen van het toepasselijke toegangsniveau.

Vul vervolgens de naam van uw Azure abonnement, de naam van de cloud-service en de naam van uw VM (verwijderen van de < en > tekens), en voer deze opdrachten.

    $subscr="<Name of your Azure subscription>"
    $serviceName="<Name of the cloud service that contains the target virtual machine>"
    $vmName="<Name of the target virtual machine>"
    .\InstallWinRMCertAzureVM.ps1 -SubscriptionName $subscr -ServiceName $serviceName -Name $vmName

U kunt de naam van de juiste abonnement krijgen van de eigenschap _SubscriptionName_ van de weergave van de opdracht **Get-AzureSubscription** . U kunt de naam van de cloud-service voor de virtuele machine krijgen van de kolom _servicenaam_ in de weergave van de opdracht **Get-AzureVM** .

Controleren of het nieuwe certificaat. Open een module Certificaten voor de huidige gebruiker en kijk in de map **Trusted Root certificering Authorities\Certificates** . Ziet u een certificaat met de DNS-naam van uw cloudservice in de kolom uitgegeven aan (voorbeeld: cloudservice4testing.cloudapp.net).

Start vervolgens een externe Azure PowerShell-sessie met behulp van deze opdrachten.

    $uri = Get-AzureWinRMUri -ServiceName $serviceName -Name $vmName
    $creds = Get-Credential
    Enter-PSSession -ConnectionUri $uri -Credential $creds

Na het invoeren van ongeldige beheerdersreferenties, ziet u een vergelijkbare naar de volgende Azure PowerShell-prompt:

    [cloudservice4testing.cloudapp.net]: PS C:\Users\User1\Documents>

Het eerste deel van deze prompt wordt weergegeven, is de naam van uw cloud-service met het doel VM, die kunnen afwijken van "cloudservice4testing.cloudapp.net". U kunt nu Azure PowerShell-opdrachten voor deze cloudservice om te kunnen onderzoeken de problemen die worden genoemd en corrigeren van de configuratie geven.

### <a name="to-manually-correct-the-remote-desktop-services-listening-tcp-port"></a>Handmatig corrigeren de Remote Desktop Services luisteren TCP-poorten

Als u niet het [pakket van Azure IaaS (Windows) diagnostische hulpprogramma's](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864) voor het probleem **RDP-connectiviteit met een Azure VM (opnieuw opstarten vereist)** op de externe Azure PowerShell-sessie vraag wordt uitgevoerd, kunt u deze opdracht uitvoeren.

    Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"

De eigenschap poortnummer ziet u het huidige poortnummer. Wijzig indien nodig het externe bureaublad poort getal terug op de standaardwaarde (3389) met behulp van deze opdracht.

    Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber" -Value 3389

Controleer of dat de poort op 3389 met behulp van deze opdracht is gewijzigd.

    Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"

De externe Azure PowerShell-sessie met behulp van deze opdracht afsluiten.

    Exit-PSSession

Controleer of het eindpunt extern bureaublad voor Azure VM is ook TCP-poorten 3398 als de interne poort. Start de VM Azure en probeer het opnieuw de verbinding met extern bureaublad.


## <a name="additional-resources"></a>Aanvullende informatie

[Azure diagnostics-pakket van IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)

[Het opnieuw instellen van een wachtwoord of de service Extern bureaublad voor Windows virtuele machines](virtual-machines-windows-reset-rdp.md)

[Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md)

[Problemen met Secure Shell (SSH)-verbindingen met Linux gebaseerde Azure virtuele machines](virtual-machines-linux-troubleshoot-ssh-connection.md)

[Problemen met toegang tot een toepassing wordt uitgevoerd op een Azure virtuele machines](virtual-machines-linux-troubleshoot-app-connection.md)
