<properties
   pageTitle="Maken van aangepaste DNS-records voor een web-app | Microsoft Azure  "
   description="Het maken van aangepaste domein DNS-records voor WebApp met Azure DNS."
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

# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a>DNS-records voor een web-app maken in een aangepast domein

U kunt Azure DNS gebruiken voor het hosten van een aangepast domein voor uw web-apps. Zoals u een Azure web-app maakt en u wilt dat gebruikers toegang toe door een contoso.com, of www.contoso.com gebruiken als een FQDN-naam.

Hiervoor hebt u twee records maken:

- Een record hoofdsite "A" is contoso.com aan te wijzen
- Een "CNAME"-record voor de www-naam die naar de A-record verwijst

Houd er rekening mee dat de A-record als handmatig als u een A-record voor een web-app in Azure wordt aangegeven zijn moet maakt, bijgewerkt als u het onderliggende IP-adres voor de web-app wijzigingen.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint, moet u eerst een DNS-zone maken in Azure DNS, en de zone delegeren in uw registrar naar Azure DNS.

1. Als u wilt maken van een DNS-zone, volg de stappen in [een DNS-zone maken](dns-getstarted-create-dnszone.md).
2. Als u wilt delegeren naar Azure DNS van uw DNS-, de stappen in de [DNS-domein delegering](dns-domain-delegation.md)uit te voeren.

Na het maken van een zone en deze overdragen aan Azure DNS, kunt u vervolgens records maken voor uw aangepaste domein.


## <a name="1-create-an-a-record-for-your-custom-domain"></a>1. een A-record voor uw aangepaste domein maken

Een A-record wordt gebruikt om een naam toewijzen aan het IP-adres. In het volgende voorbeeld wordt wordt toegewezen @ als een A-record aan een IPv4-adres:

### <a name="step-1"></a>Stap 1

Een A-record maken en toewijzen aan een variabele $rs

    $rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600

### <a name="step-2"></a>Stap 2

De waarde IPv4 toevoegen aan de eerder gemaakte recordset "@" gebruik van de variabele $rs is toegewezen. De IPv4-waarde die is toegewezen, is het IP-adres voor uw web-app.

Als u het IP-adres voor een web-app zoekt, volg de stappen in [een aangepaste domeinnaam in Azure App-Service configureren](../web-sites-custom-domain-name.md#Find-the-virtual-IP-address).

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address <your web app IP address>

### <a name="step-3"></a>Stap 3

De wijzigingen in de recordset vastleggen. Gebruik `Set-AzureRMDnsRecordSet` de wijzigingen uploaden naar de record die is ingesteld op Azure DNS:

    Set-AzureRMDnsRecordSet -RecordSet $rs

## <a name="2-create-a-cname-record-for-your-custom-domain"></a>2. Maak een CNAME-record voor uw aangepaste domein

Als uw domein wordt al beheerd door Azure-DNS (Zie [DNS-domein delegering](dns-domain-delegation.md), kunt u de volgende handelingen uit in het voorbeeld om te maken van een CNAME-record voor contoso.azurewebsites.net.

### <a name="step-1"></a>Stap 1

PowerShell openen en een nieuwe set van de CNAME-record maken en toewijzen aan een variabele $rs. In dit voorbeeld wordt een type recordset CNAME met een "time to live" maken van 600 seconden in DNS-zone naam 'contoso.com'.

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Stap 2

Nadat de CNAME-recordset is gemaakt, moet u de waarde van een alias die naar de web-app verwijst maken.

De eerder toegewezen variabele "$rs" kunt u de onderstaande PowerShell-opdracht de alias maken voor het web app-contoso.azurewebsites.net.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>Stap 3

De wijzigingen met de `Set-AzureRMDnsRecordSet` cmdlet:

    Set-AzureRMDnsRecordSet -RecordSet $rs

U kunt de record valideren juist is gemaakt door de query's uitvoeren de "www.contoso.com" met nslookup, zoals hieronder wordt weergegeven:

    PS C:\> nslookup
    Default Server:  Default
    Address:  192.168.0.1

    > www.contoso.com
    Server:  default server
    Address:  192.168.0.1

    Non-authoritative answer:
    Name:    <instance of web app service>.cloudapp.net
    Address:  <ip of web app service>
    Aliases:  www.contoso.com
    contoso.azurewebsites.net
    <instance of web app service>.vip.azurewebsites.windows.net

## <a name="create-an-awverify-record-for-web-apps"></a>Maken van een record "awverify" voor WebApps


Als u besluit het gebruik van een A-record voor uw web-app, moet u gaan via een verificatieproces om te zorgen dat u eigenaar bent van het aangepaste domein. Deze verificatiestap wordt uitgevoerd door het maken van een speciaal CNAME-record met de naam 'awverify'. Deze sectie wordt toegepast op alleen een records.


### <a name="step-1"></a>Stap 1

De "awverify"-record hebt gemaakt. We gaan in het onderstaande voorbeeld wordt de 'aweverify'-record voor contoso.com te verifiëren voor het aangepaste domein maken.

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "awverify" -RecordType "CNAME" -Ttl 600

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Stap 2

Nadat de recordset "awverify" is gemaakt, toewijzen aan de CNAME-record alias instellen. In het onderstaande voorbeeld wordt we de CNAMe-record alias ingesteld op awverify.contoso.azurewebsites.net toewijzen.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>Stap 3

De wijzigingen met de `Set-AzureRMDnsRecordSet cmdlet`, zoals in de opdracht hieronder weergegeven.

    Set-AzureRMDnsRecordSet -RecordSet $rs



## <a name="next-steps"></a>Volgende stappen

Volg de stappen in [een aangepaste domeinnaam voor App-Service configureren](../app-service-web/web-sites-custom-domain-name.md) voor het configureren van uw web-app om een aangepaste domein te gebruiken.








