<properties
   pageTitle="DNS-record sets en records op Azure DNS-met Azure CLI beheren | Microsoft Azure"
   description="Het beheren van DNS-record sets en records op DNS-Azure wanneer uw domein op Azure DNS-hostingprovider. Alle CLI-opdrachten voor bewerkingen op record sets en records."
   services="dns"
   documentationCenter="na"
   authors="jtuliani"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="jtuliani"/>

# <a name="manage-dns-records-and-record-sets-by-using-cli"></a>DNS-records en record sets beheren met behulp van CLI


> [AZURE.SELECTOR]
- [Azure-Portal](dns-operations-recordsets-portal.md)
- [Azure CLI](dns-operations-recordsets-cli.md)
- [PowerShell](dns-operations-recordsets.md)


In dit artikel leest u hoe u de record sets en records voor uw DNS-zone beheren met behulp van de platforms Azure-opdrachtregel (CLI).

Het is belangrijk is voor meer informatie over het verschil tussen de DNS-record sets en afzonderlijke DNS-records. Een recordset is een verzameling records in een zone met dezelfde naam hebben zijn hetzelfde type. Zie [lidmaatschap record sets en records](dns-getstarted-create-recordset-cli.md)voor meer informatie.


## <a name="configure-the-cross-platform-azure-cli"></a>De platforms Azure CLI configureren

Azure DNS is een Azure resourcemanager alleen-lezen-service. Een Azure-Service Management API er geen. Zorg ervoor dat de CLI Azure is geconfigureerd voor gebruik resourcemanager-modus met behulp van de `azure config mode arm` opdracht.

Als er **Fout: 'dns' is niet een azure opdracht**, hoogstwaarschijnlijk omdat u Azure CLI in de modus voor beheer van Azure-Service, niet in de modus van bronbeheer gebruikt.

## <a name="create-a-new-record-set-and-record"></a>Maak een nieuwe recordset en de record

Als u wilt maken van een nieuwe record in de portal van Azure instellen, raadpleegt u [een set record en records maken](dns-getstarted-create-recordset-cli.md).


## <a name="retrieve-a-record-set"></a>Een recordset ophalen

Als u wilt ophalen van een bestaande recordset, gebruikt u `azure network dns record-set show`. Geef de zonenaam van de, in de resourcegroep record relatieve naam en recordtype instellen. Gebruik het volgende voorbeeld wordt de waarden vervangen door uw eigen.

    azure network dns record-set show myresourcegroup contoso.com www A


## <a name="list-record-sets"></a>Lijst met records sets

U kunt alle records in een DNS-zone weergeven met behulp van de `azure network dns record-set list` opdracht. Moet u de resource groepsnaam en de zonenaam van de opgeven.

### <a name="to-list-all-record-sets"></a>Voor een overzicht van alle record sets

In dit voorbeeld geeft alle records sets, ongeacht de naam of het recordtype:

    azure network dns record-set list myresourcegroup contoso.com

### <a name="to-list-record-sets-of-a-given-type"></a>Aan de lijst record sets van een bepaald type

In dit voorbeeld geeft alle records sets die overeenkomen met het opgegeven recordtype (in dit geval 'Een' records):

    azure network dns record-set list myresourcegroup contoso.com A


## <a name="add-a-record-to-a-record-set"></a>Een record toevoegen aan een Recordset

U records toevoegen aan de record sets met behulp van de `azure network dns record-set add-record`opdracht. De parameters voor het toevoegen van records aan een recordset varieert afhankelijk van het type van de record die u instelt. Bijvoorbeeld wanneer u een set record van het type 'A' gebruikt, kunt u alleen opgeven records met de parameter `-a <IPv4 address>`.

Als u wilt maken in een recordset, gebruikt u de `azure network dns record-set create`opdracht. Geef de zonenaam van de, in de resourcegroep record relatieve naam, recordtype en tijd aan live (TTL) instellen. Als de `--ttl` parameter is niet gedefinieerd, de standaardwaarde (in seconden) vier.

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300


Nadat u de 'Een' recordset hebt gemaakt, het IPv4-adres toevoegen met behulp van de `azure network dns record-set add-record`opdracht.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1


De volgende voorbeelden wordt het maken van een set record van elk recordtype. Elke recordset bevat één record.

[AZURE.INCLUDE [dns-add-record-cli-include](../../includes/dns-add-record-cli-include.md)]


## <a name="update-a-record-in-a-record-set"></a>Een record in een recordset wordt bijgewerkt

### <a name="to-add-another-ip-address-1234-to-an-existing-a-record-set-www"></a>Als u wilt een ander IP-adres (1.2.3.4) toevoegen aan een bestaande 'Een'-record instellen ('www'):

    azure network dns record-set add-record  myresourcegroup contoso.com  A
    -a 1.2.3.4
    info:    Executing command network dns record-set add-record
    Record set name: www
    + Looking up the dns zone "contoso.com"
    + Looking up the DNS record set "www"
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/a/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/a
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:        IPv4 address                : 192.168.1.1
    data:        IPv4 address                : 1.2.3.4
    data:
    info:    network dns record-set add-record command OK

### <a name="to-remove-an-existing-value-from-a-record-set"></a>Een bestaande waarde uit een recordset verwijderen
Gebruik `azure network dns record-set delete-record`.

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 1.2.3.4
    info:    Executing command network dns record-set delete-record
    + Looking up the DNS record set "www"
    Delete DNS record? [y/n] y
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/A/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/A
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:    IPv4 address                    : 192.168.1.1
    data:
    info:    network dns record-set delete-record command OK



## <a name="remove-a-record-from-a-record-set"></a>Een record verwijderen uit een Recordset

Records kunnen worden verwijderd uit een record instellen met behulp van `azure network dns record-set delete-record`. De record die wordt verwijderd moet een exacte overeenkomst met een bestaande record over alle parameters.

De laatste record verwijdert uit een recordset verwijderd de recordset niet. Zie het gedeelte van dit artikel genoemd [een set record verwijderen](#delete)voor meer informatie.

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 192.168.1.1

    azure network dns record-set delete myresourcegroup contoso.com www A

### <a name="remove-an-aaaa-record-from-a-record-set"></a>Een record AAAA verwijderen uit een Recordset

    azure network dns record-set delete-record myresourcegroup contoso.com test-aaaa  AAAA -b "2607:f8b0:4009:1803::1005"

### <a name="remove-a-cname-record-from-a-record-set"></a>Een CNAME-record verwijderen uit een Recordset

    azure network dns record-set delete-record myresourcegroup contoso.com test-cname CNAME -c www.contoso.com


### <a name="remove-an-mx-record-from-a-record-set"></a>Een MX-record verwijderen uit een Recordset

    azure network dns record-set delete-record myresourcegroup contoso.com "@" MX -e "mail.contoso.com" -f 5

### <a name="remove-an-ns-record-from-record-set"></a>Een NS-record uit de recordset verwijderen

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-ns" NS -d "ns1.contoso.com"

### <a name="remove-a-ptr-record-from-a-record-set"></a>Een PTR-record verwijderen uit een Recordset
In dit geval ' Mijn-volgens-zone.com' volgens zones waarmee uw IP-bereik vertegenwoordigt.  Elke PTR-record instellen in deze zone overeenkomt met een IP-adres in deze IP-bereik.

    azure network dns record-set delete-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"

### <a name="remove-an-srv-record-from-a-record-set"></a>Een SRV-record verwijderen uit een Recordset

    azure network dns record-set delete-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 -w 5 -o 8080 -u "sip.contoso.com"

### <a name="remove-a-txt-record-from-a-record-set"></a>Een TXT-record verwijderen uit een Recordset

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-TXT" TXT -x "this is a TXT record"

## <a name="delete"></a>Verwijderen van een Recordset

Record sets kunnen worden verwijderd met behulp van de `Remove-AzureRmDnsRecordSet` cmdlet. U kunt de SOA niet verwijderen en NS-record die is ingesteld op de zone Top (naam = "@") die automatisch zijn gemaakt wanneer de zone is gemaakt. Ze worden automatisch worden verwijderd als de zone wordt verwijderd.

In het volgende voorbeeld wordt worden de 'Een' instellen 'test-a' record verwijderd uit de 'contoso.com' DNS-zone:

    azure network dns record-set delete myresourcegroup contoso.com  "test-a" A

De schakeloptie optioneel *-q* kan worden gebruikt om te onderdrukken bevestiging wordt gevraagd.


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over DNS Azure [Azure DNS-overzicht](dns-overview.md). Zie [maken van DNS-zones en record wilt instellen met de .NET SDK](dns-sdk.md)voor informatie over het automatiseren van DNS.

Als u werken met omgekeerde DNS-records wilt, raadpleegt u [het omgekeerde DNS-records voor uw services met de CLI Azure beheren](dns-reverse-dns-record-operations-cli.md).
