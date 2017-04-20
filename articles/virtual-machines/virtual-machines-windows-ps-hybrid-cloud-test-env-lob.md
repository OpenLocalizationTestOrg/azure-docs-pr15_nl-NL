<properties 
    pageTitle="LOB-toepassing-testomgeving | Microsoft Azure" 
    description="Informatie over het maken van een regel op het web, bedrijfstoepassing in een hybride cloud-omgeving voor IT pro- of ontwikkeling testen." 
    services="virtual-machines-windows" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="josephd"/>

# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a>Een LOB webtoepassing instellen in een hybride cloud voor testdoeleinden

In dit onderwerp wordt u stapsgewijs begeleid bij het maken van een wolk gesimuleerd hybride omgeving voor het testen van een lijn op het web van business (LOB)-toepassing die zijn ingesloten in een Microsoft Azure. Hier ziet u de resulterende configuratie.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Deze configuratie bestaat uit:

- Een gesimuleerd on-premises netwerk wordt gehost in Azure wordt aangegeven (de TestLab VNet).
- Een cross-premises virtuele netwerk die worden gehost in Azure wordt aangegeven (TestVNET).
- Een VNet-naar-VNet VPN-verbinding.
- Een web gebaseerde LOB-server, SQL server en secundaire domeincontroller in het virtuele TestVNET-netwerk.

Deze configuratie biedt een soort_jaar en algemene beginpunt van waaruit u kunt:

- Ontwikkel en test LOB-toepassingen die worden gehost op Internet Information Services (IIS) met een SQL Server-2014 databaseback-end in Azure wordt aangegeven.
- Deze gesimuleerd hybride cloudgebaseerde IT werkbelasting van tests uitvoeren.

Er zijn drie belangrijke fasen voor het instellen van deze hybride cloud-testomgeving:

1.  Stel de cloud gesimuleerd hybride omgeving.
2.  Configureer de SQL server-computer (SQL1).
3.  Configureer de LOB-server (LOB1).

De werklast vereist een Azure-abonnement. Als u een MSDN- of Visual Studio-abonnement hebt, raadpleegt u [Azure maandelijkse creditnota voor abonnees met Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

Zie de **bedrijfstoepassingen** architectuur blauwdruk bij [netwerkarchitectuurdiagrammen voor Microsoft-Software en blauwdrukken](http://msdn.microsoft.com/dn630664)voor een voorbeeld van een productie LOB-toepassing die wordt gehost in Azure wordt aangegeven.

## <a name="phase-1-set-up-the-simulated-hybrid-cloud-environment"></a>Fase 1: De cloud gesimuleerd hybride omgeving instellen

Maak de [hybride cloud-testomgeving gesimuleerd](virtual-machines-windows-ps-hybrid-cloud-test-env-sim.md). Omdat deze testomgeving niet voor de aanwezigheid van de server 1 in de Corpnet-subnet vereist is, kunt u deze afsluiten voorlopig.

Dit is uw huidige configuratie.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)
 
## <a name="phase-2-configure-the-sql-server-computer-sql1"></a>Fase 2: Configureer de SQL-servercomputer (SQL1)

Vanuit de Azure-portal, start u de computer DC2 indien nodig.

Maak vervolgens een virtuele machine voor SQL1 met deze opdrachten vanaf de Azure PowerShell-prompt naar uw lokale computer. Voordat u deze opdrachten, vul de waarden van variabelen en verwijder de < en > tekens.

    $rgName="<your resource group name>"
    $locName="<the Azure location of your resource group>"
    $saName="<your storage account name>"
    
    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName SQL1 -VMSize Standard_A4
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-SQLDataDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name "Data" -DiskSizeInGB 100 -VhdUri $vhdURI  -CreateOption empty
    
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the SQL Server computer." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SQL1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Standard -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Met de portal van Azure verbinding maken met SQL1 met de lokale beheerdersaccount van SQL1.

Configureer vervolgens Windows Firewall regels voor het toestaan van eenvoudige connectiviteit testen en SQL Server-verkeer is toegestaan. Vanaf een beheerdersniveau Windows PowerShell opdrachtprompt op SQL1, moet u deze opdrachten uitvoeren.

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

De opdracht ping moet leiden tot vier succesvolle antwoorden van IP-adres 192.168.0.4.

Voeg vervolgens de gegevensschijf extra op SQL1 als een nieuw volume met de letter F:.

1.  In het linkerdeelvenster van Serverbeheer, klik op **bestand en Storage-Services**en klik vervolgens op **schijven**.
2.  Klik in het deelvenster inhoud in de groep **schijven** **schijf 2** (met de **Partition** ingesteld op **Onbekend**).
3.  Klik op **taken**en klik vervolgens op **NieuwVolume**.
4.  Klik op het vak voor pagina van de Wizard Nieuw Volume begint, klik op **volgende**.
5.  Klik op de pagina selecteren de server en schijf **schijf 2**op en klik vervolgens op **volgende**. Wanneer u wordt gevraagd, klikt u op **OK**.
6.  Klik op **volgende**op het opgeven van de grootte van de volume-pagina.
7.  Voer op de toewijzen aan een station letter of de map pagina, klikt u op **volgende**.
8.  Klik op **volgende**op de pagina selecteert u bestand systeem-instellingen.
9.  Klik op **maken**op de pagina bevestig selecties.
10. Als alles compleet is, klikt u op **sluiten**.

Deze opdrachten uitvoeren bij de opdrachtprompt van Windows PowerShell op SQL1:

    md f:\Data
    md f:\Log
    md f:\Backup

Vervolgens SQL1 te koppelen aan het CORP Windows Server Active Directory-domein met de volgende opdrachten bij de Windows PowerShell-prompt op SQL1.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Gebruik het CORP\User1-account wanneer hierom wordt gevraagd domein accountreferenties voor de opdracht **Toevoegen-Computer** op te geven.

Met de portal van Azure start opnieuw op en verbinding maken met SQL1 *met het lokale beheerdersaccount van SQL1*.

Configureer SQL Server-2014 als u wilt het station F: gebruiken voor nieuwe databases en voor de machtigingen van de gebruiker-account.

1.  Vanuit het startscherm typt u **SQL Server Management**en klik vervolgens op **SQL Server 2014 Management Studio**.
2.  In **verbinding maken met de Server**, klikt u op **verbinding maken**.
3.  In het deelvenster van de structuurweergave Objectverkenner met de rechtermuisknop op **SQL1**en klik vervolgens op **Eigenschappen**.
4.  Klik op **Database-instellingen**in het venster **Eigenschappen van de Server** .
5.  Zoek de **Database standaardlocaties** en stel deze waarden: 
    - Voor **gegevens**, typt u het pad **f:\Data**.
    - **Log**, typ het pad **f:\Log**.
    - Typ het pad **f:\Backup**voor **back-up**.
    - Opmerking: Alleen nieuwe databases Gebruik deze locaties.
6.  Klik op **OK** om het venster te sluiten.
7.  Open in het deelvenster **Objectverkenner** structuur **beveiliging**.
8.  Met de rechtermuisknop op **aanmeldingen** en klik vervolgens op **Nieuwe aanmelding**.
9.  Typ in het vak **Login name** **CORP\User1**.
10. Klik op de pagina **Serverrollen** **systeembeheerder**op en klik vervolgens op **OK**.
11. Sluit Microsoft SQL Server Management Studio.

Dit is uw huidige configuratie.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)
 
## <a name="phase-3-configure-the-lob-server-lob1"></a>Fase 3: De LOB-server (LOB1) configureren

Maak eerst een virtuele machine voor LOB1 met deze opdrachten bij de opdrachtprompt Azure PowerShell naar uw lokale computer.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<your storage account name>"
    
    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName LOB1 -VMSize Standard_A2
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for LOB1."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName LOB1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/LOB1-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name LOB1-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Gebruik vervolgens de portal van Azure verbinding maken met LOB1 met de referenties van het lokale beheerdersaccount van LOB1.

Een regel Windows Firewall dat verkeer is toegestaan voor eenvoudige connectiviteit testen vervolgens configureren. Vanaf een beheerdersniveau Windows PowerShell opdrachtprompt op LOB1, moet u deze opdrachten uitvoeren.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

De opdracht ping moet leiden tot vier succesvolle antwoorden van IP-adres 192.168.0.4.

Vervolgens LOB1 te koppelen aan de CORP Active Directory-domein met de volgende opdrachten bij de Windows PowerShell-prompt.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Gebruik het CORP\User1-account wanneer hierom wordt gevraagd domein accountreferenties voor de opdracht **Toevoegen-Computer** op te geven.

Met de portal van Azure start opnieuw op en verbinding maken met LOB1 met de CORP\User1-account en wachtwoord.

Vervolgens LOB1 configureren voor IIS en toegang vanaf CLIENT1 testen.

1.  Via Serverbeheer, klik op **functies toevoegen en onderdelen**.
2.  Klik op **volgende**op de pagina **voordat u begint** .
3.  Klik op **volgende**op de pagina **installatietype selecteren** .
4.  Klik op **volgende**op de pagina **Selecteer doelserver** .
5.  Klik op de pagina **Serverrollen** **Webserver (IIS)** in de lijst met **rollen**.
6.  Wanneer u wordt gevraagd, op **Onderdelen toevoegen**en klik vervolgens op **volgende**.
7.  Klik op **volgende**op de pagina **onderdelen van de** .
8.  Klik op **volgende**op de pagina **Webserver (IIS)** .
9.  Klik op de pagina **Rolservices** selecteren of schakel de selectievakjes uit voor de services die u nodig hebt voor het testen van uw LOB-toepassing en klik vervolgens op **volgende**.
10. Klik op de pagina **Bevestig installatie zijn geselecteerd** , klikt u op **installeren**.
11. Wacht totdat de installatie van onderdelen is voltooid en klik vervolgens op **sluiten**.
12. Verbinding maken met de computer CLIENT1 met de CORP\User1-accountreferenties in de portal Azure en start Internet Explorer.
13. Typ **http://lob1/** in de adresbalk en druk op ENTER. Hier ziet u de standaard IIS-8-webpagina.

Dit is uw huidige configuratie.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)
 
Deze omgeving is nu klaar voor het implementeren van uw webtoepassing op LOB1 en functionaliteit uit CLIENT1 op Corpnet subnet testen.

## <a name="next-step"></a>Volgende stap

- Voeg een nieuwe virtuele machine met behulp van de [Azure-portal](virtual-machines-windows-hero-tutorial.md).
