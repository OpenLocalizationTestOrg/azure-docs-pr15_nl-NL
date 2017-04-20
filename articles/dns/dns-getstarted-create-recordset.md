<properties
   pageTitle="Maak een recordset en records voor een DNS-zone via PowerShell | Microsoft Azure"
   description="Het maken van hostrecords voor Azure DNS. Bij het instellen van de record wordt ingesteld en records via PowerShell"
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



# <a name="create-dns-record-sets-and-records-by-using-powershell"></a>DNS-record sets en records maken met behulp van PowerShell


> [AZURE.SELECTOR]
- [Azure-Portal](dns-getstarted-create-recordset-portal.md)
- [PowerShell](dns-getstarted-create-recordset.md)
- [Azure CLI](dns-getstarted-create-recordset-cli.md)

In dit artikel begeleidt u bij het proces van het maken van records en records sets met Windows PowerShell. Na het maken van uw DNS-zone, kunt u de DNS-records voor uw domein toevoegen. Hiervoor moet u eerst meer informatie over DNS-records en record sets.

[AZURE.INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## <a name="verify-that-you-have-the-latest-version-of-powershell"></a>Controleer of u de nieuwste versie van PowerShell

Controleer of u de nieuwste versie van de Azure resourcemanager PowerShell-cmdlets hebt geïnstalleerd. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor meer informatie over het installeren van de PowerShell-cmdlets.

## <a name="create-a-record-set-and-record"></a>Maken van een recordset en de record

In deze sectie wordt beschreven hoe om een recordset en de record te maken.


### <a name="1-connect-to-your-subscription"></a>1. verbinding maken met uw abonnement

Open uw PowerShell-console en verbinding maken met uw account. Gebruik in het onderstaande voorbeeld om te verbinden:

    Login-AzureRmAccount

Controleer de abonnementen voor het account.

    Get-AzureRmSubscription

Geef het abonnement waaraan u wilt gebruiken.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

Zie [Windows PowerShell gebruiken met resourcemanager](../powershell-azure-resource-manager.md)voor meer informatie over het werken met PowerShell.


### <a name="2-create-a-record-set"></a>2. een recordset maken

U record sets maken met behulp van de `New-AzureRmDnsRecordSet` cmdlet. Wanneer u een Recordset maakt, moet u de record naam, de zone, de optie time to live (TTL) en de record van het type instellen opgeven.

Om een record instellen in de top van de zone te maken (in dit geval "contoso.com"), gebruikt u de recordnaam van de "@", inclusief de aanhalingstekens. Dit is een algemene DNS-overeenkomst.

Het volgende voorbeeld wordt een record instellen met de naam van de relatieve 'www' in de DNS-Zone 'contoso.com'. De volledig gekwalificeerde naam van de recordset is "www.contoso.com". Het recordtype is "A", en de TTL is 60 seconden. Na het voltooien van deze stap, hebt u een lege 'www' record instellen die is toegewezen aan de variabele *$rs*.

    $rs = New-AzureRmDnsRecordSet -Name "www" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 60

#### <a name="if-a-record-set-already-exists"></a>Als een record ingesteld al bestaat

Als een record instellen, al bestaat, mislukt de opdracht tenzij de *-overschrijven* schakeloptie wordt gebruikt. De *-overschrijven* optie activeert een bevestiging wordt gevraagd, die kan worden onderdrukt met behulp van de *-dwingen* schakelen.


    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 -ZoneName contoso.com -ResouceGroupName MyAzureResouceGroup [-Tag $tags] [-Overwrite] [-Force]


In dit voorbeeld geeft u de zone met behulp van de zonenaam van de en de naam van de resource-groep. U kunt ook een object zone opgeven die door `Get-AzureRmDnsZone` of `New-AzureRmDnsZone`.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup
    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 –Zone $zone [-Tag $tags] [-Overwrite] [-Force]

`New-AzureRmDnsRecordSet`Geeft als resultaat een lokaal object dat de record set die is gemaakt in Azure DNS vertegenwoordigt.

### <a name="3-add-a-record"></a>3. een record toevoegen

Als u wilt gebruiken in de recordset gemaakte 'www', moet u records aan toe te voegen. U kunt IPv4 toevoegen *A* records aan de 'www'-record instellen via het volgende voorbeeld. In dit voorbeeld is afhankelijk van de variabele *$rs* die u in de vorige stap hebt ingesteld.

Records toevoegen aan een record instellen met behulp van `Add-AzureRmDnsRecordConfig` is een offline bewerking. Alleen de lokale variabele *$rs* wordt bijgewerkt.


    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.185.46
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.188.221

### <a name="4-commit-the-changes"></a>4. de wijzigingen doorvoeren

De wijzigingen in de recordset vastleggen. Gebruik `Set-AzureRmDnsRecordSet` de wijzigingen uploaden naar de record die is ingesteld op Azure DNS.

    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="5-retrieve-the-record-set"></a>5. de recordset ophalen

U kunt de record van Azure DNS instellen met behulp van ophalen `Get-AzureRmDnsRecordSet` zoals wordt weergegeven in het volgende voorbeeld.


    Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup


    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : MyAzureResourceGroup
    Ttl               : 3600
    Etag              : 68e78da2-4d74-413e-8c3d-331ca48246d9
    RecordType        : A
    Records           : {134.170.185.46, 134.170.188.221}
    Tags              : {}


U kunt ook het hulpprogramma nslookup of andere DNS-hulpprogramma's gebruiken om query's in de nieuwe recordset.

Als u het domein naar de naamservers van Azure DNS-nog niet hebt overgedragen, moet u de naam, de server en het adres van uw zone expliciet op te geven.


    nslookup www.contoso.com ns1-01.azure-dns.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    Name:    www.contoso.com
    Addresses:  134.170.185.46
                134.170.188.221

## <a name="create-a-record-set-of-each-type-with-a-single-record"></a>Een set record van elk type maken met een enkele record


De volgende voorbeelden wordt het maken van een set record van elk recordtype. Elke recordset bevat één record.

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]


## <a name="next-steps"></a>Volgende stappen

[Het beheren van DNS-zones via PowerShell](dns-operations-dnszones.md)

[DNS-records beheert en record wordt ingesteld via PowerShell](dns-operations-recordsets.md)

[Azure bewerkingen met .NET SDK automatiseren](dns-sdk.md)
