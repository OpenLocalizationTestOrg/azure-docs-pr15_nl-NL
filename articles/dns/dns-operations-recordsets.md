<properties
   pageTitle="DNS-record sets en records beheren met behulp van de Azure portal | Microsoft Azure"
   description="Het beheren van DNS-record sets en records op DNS-Azure wanneer uw domein op Azure DNS-hostingprovider. Alle PowerShell-opdrachten voor bewerkingen op record sets en records."
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

# <a name="manage-dns-records-and-record-sets-by-using-powershell"></a>DNS-records beheert en record wordt ingesteld via PowerShell



> [AZURE.SELECTOR]
- [Azure-Portal](dns-operations-recordsets-portal.md)
- [Azure CLI](dns-operations-recordsets-cli.md)
- [PowerShell](dns-operations-recordsets.md)



In dit artikel leest u hoe u de record sets en records voor uw DNS-zone beheren met behulp van Windows PowerShell.

Het is belangrijk is voor meer informatie over het verschil tussen de DNS-record sets en afzonderlijke DNS-records. Een recordset ziet u de lijst met records in een zone met dezelfde naam hebben zijn hetzelfde type. Zie [maken DNS-record sets en records met behulp van de Azure-portal](dns-getstarted-create-recordset-portal.md)voor meer informatie.

Als u wilt uw record sets en records beheren, moet u de nieuwste versie van de Azure resourcemanager PowerShell-cmdlets. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md)voor meer informatie. Zie voor meer informatie over het werken met PowerShell [Azure PowerShell gebruiken met Azure Resource Manager](../powershell-azure-resource-manager.md).



## <a name="create-a-new-record-set-and-a-record"></a>Een nieuwe recordset en een record maken

Als u wilt vastleggen instellen via PowerShell, Zie [maken DNS-record sets en records via PowerShell](dns-getstarted-create-recordset.md).

## <a name="get-a-record-set"></a>Een recordset ophalen

Als u wilt ophalen van een bestaande recordset, gebruikt u `Get-AzureRmDnsRecordSet`. Geef dat de record relatieve naam, het recordtype en de zone instellen.

    $rs = Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

Net als bij `New-AzureRmDnsRecordSet`, de recordnaam moet een relatieve naam, wat betekent dit de zonenaam van de moet uitsluiten.

Met behulp van de zonenaam van de en de naam van de resource-groep of met behulp van een zone-object, kunt u de zone opgeven:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $rs = Get-AzureRmDnsRecordSet -Name www –RecordType A -Zone $zone

`Get-AzureRmDnsRecordSet`Geeft als resultaat een lokaal object dat de record set die is gemaakt in Azure DNS vertegenwoordigt.

## <a name="list-record-sets"></a>Lijst met records sets

U kunt ook`Get-AzureRmDnsRecordSet` ingesteld op lijst in de record wordt als u weglaat de *– naam* en/of *– RecordType* parameters.

### <a name="to-list-all-record-sets"></a>Voor een overzicht van alle record sets

In dit voorbeeld geeft alle records sets, ongeacht de naam of het recordtype:

    $list = Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-list-record-sets-of-a-given-record-type"></a>Aan de lijst record sets met een bepaald recordtype

In dit voorbeeld geeft alle records sets die overeenkomen met het opgegeven recordtype. In dit geval geretourneerd de record die is ingesteld dat wil zeggen de 'Een' records is:

    $list = Get-AzureRmDnsRecordSet –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

De zone u kunt opgeven met behulp van de *– Zonenaam* en *– ResourceGroupName* parameters (zoals) of door op te geven van een zone-object:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $list = Get-AzureRmDnsRecordSet -Zone $zone

## <a name="add-a-record-to-a-record-set"></a>Een record toevoegen aan een Recordset

U records toevoegen aan de record sets met behulp van de `Add-AzureRmDnsRecordConfig` cmdlet. Dit is een offline bewerking. Alleen het lokale object dat de recordset vertegenwoordigt wordt gewijzigd.

De parameters voor het toevoegen van records aan een recordset varieert afhankelijk van het type van de recordset. Bijvoorbeeld wanneer u met een set record van het type "A", kunt u alleen opgeven records met de parameter *-IPv4Address*.

Als u meer records kunnen worden toegevoegd aan elke record die is ingesteld door extra oproepen naar `Add-AzureRmDnsRecordConfig`. U kunt maximaal 20 records toevoegen aan een recordset. Echter record sets van het type "CNAME" kunnen maximaal één record bevatten en een recordset mogen geen twee identieke records bevatten. Lege record sets (met nul records) kunnen worden gemaakt, maar worden niet weergegeven op de Azure DNS-naamservers.

Nadat de recordset de gewenste verzameling records bevat, moet u deze doorvoeren met behulp van de `Set-AzureRmDnsRecordSet` cmdlet. Nadat een recordset vastgelegd is, vervangt de bestaande record instellen in Azure DNS.

### <a name="to-create-an-a-record-set-with-a-single-record"></a>Maken van een A-record instellen met een enkele record

    $rs = New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

*Omgeleide*, wat betekent dat u het object recordset doorgeven door het sluisteken gebruiken in plaats van deze wordt doorgegeven als een parameter kan ook worden door de volgorde van bewerkingen om een record te maken. Bijvoorbeeld:

    New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Add-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="additional-record-type-examples"></a>Voorbeelden van de extra record van het type

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]

## <a name="modify-existing-record-sets"></a>Bestaande record groepen wijzigen

De stappen voor het wijzigen van een bestaande recordset zijn vergelijkbaar met de stappen die u neemt bij het maken van records. De volgorde van bewerkingen is als volgt:

1.  Ophalen van de bestaande record instellen met behulp van `Get-AzureRmDnsRecordSet`.

2.  De record die is ingesteld door op records toe te voegen records, te verwijderen als u de recordparameters wijzigen of tijd wijzigen van de record worden ingesteld op live (TTL). Dit is een offline bewerking. Alleen het lokale object dat de recordset vertegenwoordigt wordt gewijzigd.

3.  Uw wijzigingen doorvoeren met behulp van de `Set-AzureRmDnsRecordSet` cmdlet. Deze optie vervangt de bestaande record instellen in Azure DNS.


### <a name="to-update-a-record-in-an-existing-record-set"></a>Een record in een bestaande recordset te werken

In dit voorbeeld wordt het IP-adres van een bestaande 'Een' record wijzigen:

    $rs = Get-AzureRmDnsRecordSet -name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Ipv4Address = "134.170.185.46"
    Set-AzureRmDnsRecordSet -RecordSet $rs

De `Set-AzureRmDnsRecordSet` cmdlet etag controles gebruikt om ervoor te zorgen dat wijzigingen worden niet overschreven. Gebruik de *-overschrijven* vlag deze controles onderdrukken. Zie voor meer informatie [over etags en labels](dns-getstarted-create-dnszone.md#tagetag).

### <a name="to-modify-an-soa-record"></a>Een record SOA wijzigen

U niet kunt toevoegen of verwijderen van records uit de automatisch gemaakte SOA-record instellen op de zone Top (naam = "@"). Echter, kunt u een van de parameters binnen de SOA-record (behalve "Host") en de record set TTL.

Het volgende voorbeeld ziet hoe u kunt wijzigen van de eigenschap *E-mail* van de SOA-record:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Email = "admin.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-modify-ns-records-at-the-zone-apex"></a>NS-records op de top zone te wijzigen

U kunt geen toevoegen, verwijderen of wijzigen van de records in de automatisch gemaakte NS Recordset op de zone-Top (naam = "@"). De enige wijziging dat wel toegestaan is voor het wijzigen van de recordset TTL.

Het volgende voorbeeld ziet u hoe de TTL-eigenschap van de NS-recordset wijzigen:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Ttl = 300
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-add-records-to-an-existing-record-set"></a>Records toevoegen aan een bestaande recordset

In dit voorbeeld toevoegen we twee aanvullende MX-records aan de bestaande record set:

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail2.contoso.com" -Preference 10
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail3.contoso.com" -Preference 20
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="remove-a-record-from-an-existing-record-set"></a>Een record verwijderen uit een bestaande recordset

Records kunnen worden verwijderd uit een record instellen met behulp van `Remove-AzureRmDnsRecordConfig`. De record die wordt verwijderd moet een exacte overeenkomst met een bestaande record over alle parameters. Wijzigingen moeten worden doorgevoerd met behulp van `Set-AzureRmDnsRecordSet`.

De laatste record verwijdert uit een recordset verwijderd de recordset niet. Zie [een set record verwijderen](#delete-a-record-set) hieronder voor meer informatie.


    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

De volgorde van bewerkingen een record verwijderen uit een recordset kan ook worden doorgegeven, wat betekent dat u het object recordset doorgeven door het sluisteken gebruiken in plaats van deze als een parameter wordt doorgegeven. Bijvoorbeeld:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="remove-an-aaaa-record-from-a-record-set"></a>Een record AAAA verwijderen uit een Recordset

    $rs = Get-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv6Address "2607:f8b0:4009:1803::1005"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-cname-record-from-a-record-set"></a>Een CNAME-record verwijderen uit een Recordset

Aangezien een CNAME-Recordset maximaal één record bevatten kan, verlaat als u deze record een lege recordset.

    $rs =  Get-AzureRmDnsRecordSet -name "test-cname" -RecordType CNAME -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Cname "www.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-mx-record-from-a-record-set"></a>Een MX-record verwijderen uit een Recordset

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail.contoso.com" -Preference 5
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-ns-record-from-record-set"></a>Een NS-record uit de recordset verwijderen

    $rs = Get-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname "ns1.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-srv-record-from-a-record-set"></a>Een SRV-record verwijderen uit een Recordset

    $rs = Get-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs –Priority 0 –Weight 5 –Port 8080 –Target "sip.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-txt-record-from-a-record-set"></a>Een TXT-record verwijderen uit een Recordset

    $rs = Get-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Value "This is a TXT record"
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="delete-a-record-set"></a>Verwijderen van een Recordset

Record sets kunnen worden verwijderd met behulp van de `Remove-AzureRmDnsRecordSet` cmdlet. U kunt de SOA niet verwijderen en NS-record die is ingesteld op de zone Top (naam = "@") die automatisch zijn gemaakt wanneer de zone is gemaakt. Ze worden automatisch worden verwijderd als de zone wordt verwijderd.

Gebruik een van de volgende drie manieren om een recordset verwijderen:

### <a name="specify-all-the-parameters-by-name"></a>Alle parameters opgeven met hun naam weergeven

Het optionele *-dwingen* schakeloptie kan worden gebruikt om te onderdrukken bevestiging wordt gevraagd.

    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]


### <a name="specify-the-record-set-by-name-and-type-and-specify-the-zone-by-object"></a>Geef de recordset door de naam en type en geeft u de zone door object

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Zone $zone [-Force]

### <a name="specify-the-record-set-by-object"></a>De record die is ingesteld door het object opgeven

    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet –RecordSet $rs [-Overwrite] [-Force]

Wanneer u de record instellen met behulp van een object opgeeft, kunt etag controles om ervoor te zorgen dat gelijktijdige wijzigingen niet worden verwijderd. Het optionele *-overschrijven* vlag onderdrukt deze controles. Zie [Etags en labels](dns-getstarted-create-dnszone.md#tagetag) voor meer informatie.

Het object Recordset kan ook worden doorgegeven in plaats van wordt doorgegeven als een parameter:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordSet [-Overwrite] [-Force]

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over DNS Azure [Azure DNS-overzicht](dns-overview.md). Zie [maken van DNS-zones en record wilt instellen met de .NET SDK](dns-sdk.md)voor informatie over het automatiseren van DNS.

Zie voor meer informatie over het omgekeerde DNS-records [beheren omgekeerde DNS-records voor uw services die via PowerShell](dns-reverse-dns-record-operations-ps.md).
