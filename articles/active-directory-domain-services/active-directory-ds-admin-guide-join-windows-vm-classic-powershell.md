<properties
    pageTitle="Azure Active Directory Domain Services: Handleiding voor het beheer | Microsoft Azure"
    description="Deelnemen aan een virtuele Windows-computer naar een beheerde domein met Azure PowerShell en het implementatiemodel klassieke."
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="maheshu"/>


# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-powershell"></a>Een Windows Server virtuele machine te koppelen aan een beheerde domein via PowerShell

> [AZURE.SELECTOR]
- [Azure klassieke portal - Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [PowerShell - Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

> [AZURE.IMPORTANT] Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [resourcemanager en klassiek](../resource-manager-deployment-model.md). In dit artikel beschreven hoe u met het implementatiemodel klassieke. Azure AD-domeinservices ondersteunt momenteel niet het model resourcemanager.

Volgende stappen uit hoe u een reeks Azure PowerShell-opdrachten die maken en vooraf op basis van Windows Azure virtuele machines configureren met behulp van een benadering van de bouwsteen aanpassen. Deze stappen kunt u een Windows Azure virtuele machine maken en toevoegen aan een beheerde domein van Azure AD Domain Services.

Deze stappen volgt een aanpak aanvullen-in-het-lege waarden voor het maken van Azure PowerShell-opdrachtsets. Deze methode is handig als u nog niet eerder met PowerShell of als u weten welke waarden u wilt moet opgeven voor geslaagde configuratie. Geavanceerde PowerShell-gebruikers kunnen de opdrachten uitvoeren en hun eigen waarden voor de variabelen vervangen (de regels die beginnen met '$').

Als u dit nog niet hebt gedaan, gebruikt u de instructies in [installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Azure PowerShell installeren op uw lokale computer. Open vervolgens Windows PowerShell vanaf de opdrachtprompt.

## <a name="step-1-add-your-account"></a>Stap 1: Uw account toevoegen

1. Klik bij de prompt PowerShell Typ **Toevoegen-AzureAccount** en klikt u op **Enter**.
2. Typ in het e-mailadres dat is gekoppeld aan uw Azure abonnement en klik op **Doorgaan**.
3. Typ in het wachtwoord voor uw account.
4. Klik op **aanmelden**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>Stap 2: Uw abonnement en opslag-account instellen

Instellen opslag-account en uw Azure-abonnement door deze opdrachten bij de opdrachtprompt van Windows PowerShell uit te voeren. Vervang alles in de aanhalingstekens, inclusief de < en > tekens op, met de juiste namen.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

U kunt de naam van de juiste abonnement krijgen van de eigenschap SubscriptionName van de uitvoer van de opdracht **Get-AzureSubscription** . U kunt de juiste opslagaccountnaam krijgen van de eigenschap Label van de uitvoer van de opdracht **Get-AzureStorageAccount** nadat u de opdracht **Selecteren-AzureSubscription** uitgevoerd.


## <a name="step-3-step-by-step-walkthrough---provision-the-virtual-machine-and-join-it-to-the-managed-domain"></a>Stap 3: Stapsgewijze - inrichten van de virtuele machine en deze toevoegen aan het beheerde domein
Hier ziet u de bijbehorende Azure PowerShell-opdracht te maken van deze virtuele machine, met lege regels tussen elk blok voor leesbaarheid.

Geef de informatie over de virtuele Windows-computer om te worden deze is ingericht.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

Zie voor de waarden van de InstanceSize voor D-, DS- of G-serie virtuele machines, [virtuele Machine en Cloud Service grootten voor Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

Bevatten informatie over het beheerde domein.

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

Geef de naam van de cloudservice.

    $svcname="Contoso100-test"

Geef de naam van het virtuele netwerk waaraan de VM moet worden toegevoegd. Zorg ervoor dat het beheerde AAD-DS-domein beschikbaar in deze virtuele netwerk is.

    $vnetname="MyPreviewVnet"

Selecteer de afbeelding VM moet worden gebruikt voor het inrichten van de VM.

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

Configureer de VM - set VM naam, de grootte van de exemplaar en de afbeelding wordt gebruikt.

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Lokale beheerdersreferenties voor VM aanvragen. Kies een sterk lokale beheerderswachtwoord.

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

Referenties voor een gebruikersaccount van ' AAD domeincontroller ' beheerdersgroep VM toevoegen aan de beheerde domein aanvragen. Geef geen de naam van het domein - bijvoorbeeld in ons voorbeeld, we 'bob' opgeven als de gebruikersnaam in te voeren.

    $domainadmincred=Get-Credential –Message "Now type the name (DO NOT INCLUDE THE DOMAIN) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

De VM configureren - domein join vereiste & vereiste referenties opgeven.

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

Stel een subnet voor VM.

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

Optioneel: Wijst u het IP-adres van het domein. Als u de IP-adressen van de Azure AD-domeinservices beheerde domein moeten de DNS-servers voor het virtuele netwerk instelt, is deze stap niet vereist.

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

Nu inrichten van de Windows domein behoren VM.

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-to-provision-a-windows-vm-and-automatically-join-it-to-an-aad-domain-services-managed-domain"></a>Script om inrichten van een Windows-VM en automatisch toevoegen aan een beheerde domeinservices AAD-domein
Deze waarde PowerShell-opdracht maakt een virtuele machine voor een LOB-server die:

- Maakt gebruik van de afbeelding van Windows Server 2012 R2 Datacenter.
- Een extra kleine VM is.
- De naam van de contoso-toets heeft.
- Automatisch is domein lid van de contoso100 beheerde domein.
- Wordt toegevoegd aan hetzelfde virtuele netwerk als het beheerde domein.

Hier ziet u het volledige voorbeeldscript maken van de virtuele Windows-computer en deze automatisch te koppelen aan de beheerde domeinservices van Azure AD-domein.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

    $domainadmincred=Get-Credential –Message "Now type the name (not including the domain) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a>Verwante onderwerpen
- [Azure AD-domeinservices - handleiding aan de slag](./active-directory-ds-getting-started.md)

- [Een beheerde domein van Azure AD Domain Services beheren](./active-directory-ds-admin-guide-administer-domain.md)
