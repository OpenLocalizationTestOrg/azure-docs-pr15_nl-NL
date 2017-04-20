<properties
   pageTitle="Beheren van DNS-zones via PowerShell | Microsoft Azure"
   description="U kunt DNS-zones via Azure Powershell beheren. Hoe bijwerken, verwijderen en DNS-zones op Azure DNS maken"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="how-to-manage-dns-zones-using-powershell"></a>Het beheren van DNS-Zones via PowerShell

> [AZURE.SELECTOR]
- [Azure CLI](dns-operations-dnszones-cli.md)
- [PowerShell](dns-operations-dnszones.md)



In dit artikel wordt uitgelegd hoe u uw DNS-zone beheren via PowerShell. Pas deze stappen gebruiken, moet u de nieuwste versie van de Azure resourcemanager PowerShell-cmdlets (1,0 of later) installeren. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor meer informatie over het installeren van de PowerShell-cmdlets.


## <a name="create-a-new-dns-zone"></a>Een nieuwe DNS-zone maken

Een DNS-zone, Zie [maken een DNS-zone via PowerShell](dns-getstarted-create-dnszone.md).

## <a name="get-a-dns-zone"></a>Een DNS-zone ophalen

Als u wilt ophalen een DNS-zone, gebruikt u de `Get-AzureRmDnsZone` cmdlet. Deze bewerking retourneert een DNS-zone-object die overeenkomt met een bestaande zone in Azure DNS. Het object gegevens over de zone (zoals het aantal records sets) bevat, maar geen bevat de paren record zelf.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup

## <a name="list-dns-zones"></a>Lijst met DNS-zones

Door de zonenaam van de uit weg te laten `Get-AzureRmDnsZone`, kunt u alle zones in een resourcegroep opsommen. Deze bewerking geeft als resultaat een matrix van zone objecten.

    $zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup

## <a name="update-a-dns-zone"></a>Een DNS-zone bijwerken

Een DNS-zone resource kan worden gewijzigd met behulp van `Set-AzureRmDnsZone`. Hiermee wordt een van de DNS-record sets binnen de zone niet bijgewerkt (Zie [het beheren van DNS-records](dns-operations-recordsets.md)). Het wordt alleen gebruikt om de eigenschappen van de resource zone zelf te werken. Dit is momenteel alleen naar de resourcemanager van Azure 'tags' voor de resource zone. Zie [Etags en labels](dns-getstarted-create-dnszone.md#Etags-and-tags) voor meer informatie.

Gebruik een van de volgende twee manieren om te update DNS-zone:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a>Geef de zone met behulp van de zone en de bron-groep

    Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Tag $tags]

### <a name="specify-the-zone-using-a-zone-object"></a>De zone met behulp van een object $zone opgeven

Geef de zone met behulp van een object $zone uit `Get-AzureRmDnsZone`. Bij gebruik van `Set-AzureRmDnsZone` met een object $zone Etag controles wordt gebruikt om ervoor te zorgen gelijktijdige wijzigingen worden niet overschreven. U kunt de optionele *-overschrijven* overschakelen naar deze controles onderdrukken. Zie [Etags en labels](dns-getstarted-create-dnszone.md#Etags-and-tags) voor meer informatie.


    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    <..modify $zone.Tags here...>
    Set-AzureRmDnsZone -Zone $zone [-Overwrite]


## <a name="delete-a-dns-zone"></a>Een DNS-Zone verwijderen

DNS-zones kunnen worden verwijderd met de cmdlet verwijderen-AzureRmDnsZone.

Voordat u verwijdert een DNS-zone in Azure DNS, moet u alle records sets, behalve voor de NS- en SOA-records in de hoofdmap van de zone die automatisch zijn gemaakt wanneer de zone is gemaakt verwijderen.

Gebruik een van de volgende twee manieren om een DNS-zone verwijderen:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>Geef de zone met behulp van de zonenaam en de naam van de resource-groep

Deze bewerking is een optionele *-dwingen* veranderen waarin de prompt om te bevestigen u wilt verwijderen van de DNS-zone onderdrukt.

    Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]

### <a name="specify-the-zone-using-a-zone-object"></a>De zone met behulp van een object $zone opgeven

Geef de zone met behulp van een object $zone uit `Get-AzureRmDnsZone`. Deze bewerking is een optionele *-dwingen* veranderen waarin de prompt om te bevestigen u wilt verwijderen van de DNS-zone onderdrukt. Net als bij `Set-AzureRmDnsZone`, geven de zone met behulp van een object $zone schakelt Etag controles om ervoor te zorgen gelijktijdige wijzigingen worden niet verwijderd. <BR>
Het optionele *-overschrijven* vlag onderdrukt deze controles. Zie [Etags en labels](dns-getstarted-create-dnszone.md#Etags-and-tags) voor meer informatie.

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsZone -Zone $zone [-Force] [-Overwrite]



De zone-object kan ook worden doorgegeven in plaats van wordt doorgegeven als een parameter:

    Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone [-Force] [-Overwrite]

## <a name="next-steps"></a>Volgende stappen

Na het maken van een DNS-zone, maken [Recordsets en records](dns-getstarted-create-recordset.md) om te beginnen met het omzetten van namen voor uw domein Internet.