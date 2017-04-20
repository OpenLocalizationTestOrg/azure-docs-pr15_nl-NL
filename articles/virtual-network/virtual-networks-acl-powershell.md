<properties
   pageTitle="Het beheren van toegangsbeheerlijsten (ACL's) voor eindpunten via PowerShell"
   description="Informatie over het beheren van ACL's met PowerShell"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-manage-access-control-lists-acls-for-endpoints-by-using-powershell"></a>Het beheren van toegangsbeheerlijsten (ACL's) voor eindpunten via PowerShell

U kunt maken en beheren netwerk toegangsbeheerlijsten (ACL's) in voor de eindpunten, met behulp van Azure PowerShell of in de beheerportal. In dit onderwerp vindt u procedures voor ACL algemene taken die u kunt uitvoeren via PowerShell. Zie [Cmdlets voor gebruikersbeheer van Azure](http://go.microsoft.com/fwlink/?LinkId=317721)voor de lijst met Azure PowerShell-cmdlets. Zie voor meer informatie over ACL's, [Wat is een netwerk Access (Toegangsbeheerlijst)?](virtual-networks-acl.md). Als u uw ACL's beheren wilt met behulp van de beheerportal, raadpleegt u [hoe u omhoog eindpunten instellen met een virtuele Machine](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

## <a name="manage-network-acls-by-using-azure-powershell"></a>Netwerk ACL's beheren met behulp van Azure PowerShell

U kunt Azure PowerShell-cmdlets gebruiken om te maken, verwijderen en te configureren (ingesteld) netwerk toegangsbeheerlijsten (ACL's). We hebt een paar voorbeelden van een aantal manieren die kunt u een ACL via PowerShell opgenomen.

Als u wilt ophalen van een volledige lijst van de ACL PowerShell-cmdlets, kunt u een van de volgende gebruiken:

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a>Een netwerk ACL met regels die toegang toestaan van een extern subnet maken

Het volgende voorbeeld ziet u een manier om te maken van een nieuwe ACL die regels bevat. Deze ACL vervolgens wordt toegepast op een virtuele machine-eindpunt. De ACL regels in het onderstaande voorbeeld wordt de toegang van een extern subnet toestaan. Een nieuwe netwerk ACL maken met een vergunning aan te vragen regels voor een extern subnet, een filter wissen van Azure te openen. Kopiëren en plak het script onder het script configureren met uw eigen waarden en vervolgens het script uitvoeren.

1. Het nieuwe netwerk ACL-object maken.

        $acl1 = New-AzureAclConfig

1. Een regel instellen waarmee toestemming geeft toegang vanuit een extern subnet dat. In het onderstaande voorbeeld stelt u regel *100* (dat heeft voorrang op regel 200 en hoger) toe te staan dat externe subnet *10.0.0.0/8* toegang tot het eindpunt VM. De waarden vervangen door uw eigen configuratievereisten voor de. De naam 'SharePoint ACL config' moet worden vervangen door de beschrijvende naam die u wilt bellen met deze regel.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"

1. Voor aanvullende regels, herhaalt u de cmdlet, de waarden vervangen door uw eigen configuratievereisten voor de. Zorg ervoor dat u de regel wijzigen volgorde aan de volgorde waarin u de regels worden toegepast wilt nummeren. Het laagste getal in de regel heeft voorrang op een hoger nummer.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"

1. Vervolgens kunt u een nieuw (toevoegen)-eindpunt maken of de ACL voor een bestaande eindpunt (ingesteld) instellen. In dit voorbeeld wordt er een nieuw VM-eindpunt genoemd 'web' toevoegen en het eindpunt VM bijwerken met de ACL-instellingen.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM

1. Vervolgens de cmdlets samenvoegen en het script uitvoeren. In dit voorbeeld er de gecombineerde cmdlets als volgt:

        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "Sharepoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a>Een regel netwerk ACL waarmee de toegang van een extern subnet verwijderen

Het volgende voorbeeld ziet u een manier om het verwijderen van een netwerk ACL regel.  Als u een regel netwerk ACL met vergunning aan te vragen regels voor een extern subnet, opent u een Azure filter wissen. Kopiëren en plak het script onder het script configureren met uw eigen waarden en vervolgens het script uitvoeren.

1. Eerste stap is het netwerk ACL-object voor het eindpunt VM ophalen. Vervolgens verwijdert u de regel ACL. In dit geval we het wilt verwijderen door de regel-ID. Hiermee wordt de regel-ID 0 alleen verwijderd uit de Toegangsbeheerlijst. Deze wordt het object ACL niet verwijderd uit het eindpunt VM.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1

1. Vervolgens moet u het object netwerk ACL toepassen op het eindpunt VM en de virtuele machine bijwerken.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a>Een netwerk ACL verwijderen uit een VM-eindpunt

In bepaalde gevallen wilt u mogelijk een netwerk ACL-object verwijderen uit een VM-eindpunt. Open hiervoor een Azure filter wissen. Kopiëren en plak het script onder het script configureren met uw eigen waarden en vervolgens het script uitvoeren.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a>Volgende stappen

[Wat is een netwerk Access (Toegangsbeheerlijst)?](virtual-networks-acl.md)
