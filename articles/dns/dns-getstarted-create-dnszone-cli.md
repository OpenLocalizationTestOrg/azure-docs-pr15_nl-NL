<properties
   pageTitle="Maken van een DNS-zone met CLI | Microsoft Azure"
   description="Informatie over het maken van DNS-zones voor Azure DNS stapsgewijze om te beginnen met het hosten van uw DNS-domein CLI gebruiken"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-an-azure-dns-zone-using-cli"></a>Een Azure DNS-zone met CLI maken


> [AZURE.SELECTOR]
- [Azure-Portal](dns-getstarted-create-dnszone-portal.md)
- [PowerShell](dns-getstarted-create-dnszone.md)
- [Azure CLI](dns-getstarted-create-dnszone-cli.md)


In dit artikel begeleidt u door de stappen voor het maken van een DNS-zone met behulp van CLI. U kunt ook een DNS-zone via PowerShell of de Azure-portal maken.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]


## <a name="before-you-begin"></a>Voordat u begint

Deze instructies gebruik Microsoft Azure CLI. Zorg ervoor dat u bijwerkt naar de meest recente Azure CLI (0.9.8 of hoger) gebruik van de DNS-Azure-opdrachten. Type `azure -v` om te controleren welke versie van Azure CLI momenteel op uw computer is geïnstalleerd.

## <a name="step-1---set-up-azure-cli"></a>Stap 1: Azure CLI instellen

### <a name="1-install-azure-cli"></a>1. Azure CLI installeren

U kunt installeren Azure CLI voor Windows, Linux of MAC. De volgende stappen moeten worden voltooid voordat u kunt met Azure CLI Azure-DNS beheren. Meer informatie vindt bij het [installeren van de Azure CLI](../xplat-cli-install.md). De DNS-opdrachten vereisen Azure CLI versie 0.9.8 of hoger.

Alle opdrachten voor netwerk-provider op CLI kunnen worden gevonden met de volgende opdracht:

    azure network

### <a name="2-switch-cli-mode"></a>2. Schakel over CLI modus

Azure DNS resourcemanager Azure gebruikt. Zorg ervoor dat u overschakelt CLI modus als ARM opdrachten wilt gebruiken.

    azure config mode arm

### <a name="3-sign-in-to-your-azure-account"></a>3. Meld u aan bij uw Azure-account

U wordt gevraagd om te verifiëren met uw referenties. Houd er rekening mee dat u alleen ORGID accounts kunt gebruiken.

    azure login -u "username"

### <a name="4-select-the-subscription"></a>4. Selecteer het abonnement

Kies welke van uw Azure-abonnementen te gebruiken.

    azure account set "subscription name"

### <a name="5-create-a-resource-group"></a>5. een resourcegroep maken

Azure resourcemanager is vereist dat alle resourcegroepen Geef een locatie. Dit wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Echter omdat alle DNS-resources globale, niet regionale zijn, heeft de keuze van resource groep locatie geen invloed op Azure DNS.

Als u een bestaande resourcegroep gebruikt, kunt u deze stap overslaan.

    azure group create -n myresourcegroup --location "West US"


### <a name="6-register"></a>6. register

De service Azure DNS wordt beheerd door de provider van de resource Microsoft.Network. Uw abonnement op Azure moet worden geregistreerd voor het gebruik van deze resource-provider voordat u Azure DNS kunt gebruiken. Dit is een eenmalige bewerking voor elk abonnement.

    azure provider register --namespace Microsoft.Network


## <a name="step-2---create-a-dns-zone"></a>Stap 2: een DNS-zone maken

Een DNS-zone is gemaakt met de `azure network dns zone create` opdracht. U kunt desgewenst een DNS-zone samen met tags maken. Labels zijn van een lijst met naam-waardeparen en door Azure Resource Manager naar label bronnen voor factuur- of groepeerniveau worden gebruikt. Zie voor meer informatie over de labels, [werken met tags ordenen van uw Azure resources](../resource-group-using-tags.md).

In Azure DNS zonenamen moeten worden opgegeven zonder een beëindiging **'.'**. Bijvoorbeeld als '**contoso.com**' plaats '**contoso.com.**'.


### <a name="to-create-a-dns-zone"></a>Een DNS-zone maken

Het volgende voorbeeld wordt gemaakt van een DNS-zone *contoso.com* genoemd in de resourcegroep *MyResourceGroup*genoemd.

Het voorbeeld gebruiken om uw DNS-zone, wordt vervangen door de waarden voor uw eigen te maken.

    azure network dns zone create myresourcegroup contoso.com

### <a name="to-create-a-dns-zone-and-tags"></a>Een DNS-zone en labels maken.

Azure DNS CLI ondersteunt tags van DNS-zones opgegeven met behulp van het optionele *-Tag* parameter. Het volgende voorbeeld ziet u hoe u projecten maken van een DNS-zone met twee labels, = demo en envelop = test.

In het onderstaande voorbeeld gebruiken om een DNS-zone en labels, wordt vervangen door de waarden voor uw eigen te maken.

    azure network dns zone create myresourcegroup contoso.com -t "project=demo";"env=test"

## <a name="view-records"></a>Records weergeven

De volgende DNS-records maken van een DNS-zone ook worden gemaakt:

- De record ' beginnen met ' (Start of Authority). Dit is aanwezig zijn in de hoofdmap van elke DNS-zone.

- De naamserver serverrecords (Domeinnaamserver). Deze weergeven in welke naamservers host de zone. Azure DNS maakt gebruik van een groep naamservers en dus andere naamservers kunnen worden toegewezen aan verschillende zones in Azure DNS. Zie [gemachtigde een domein aan Azure DNS](dns-domain-delegation.md) voor meer informatie.

Gebruik deze records wilt weergeven, `azure network dns-record-set show`.<BR>
*Syntaxis: netwerk-dns-record instellen weergeven < resourcegroep >< dns-zone-name > <name><type>*


Klik in het onderstaande voorbeeld als u de opdracht met resource groep *myresourcegroup uitvoeren*, record instellen naam *"@"* (voor een record hoofdsite), en typ *SOA*, resulteert dit in het volgende resultaat:


    azure network dns record-set show myresourcegroup "contoso.com" "@" SOA
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/SOA/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/SOA
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    SOA record:
    data:      Email                         : msnhst.microsoft.com
    data:      Expire time                   : 604800
    data:      Host                          : edge1.azuredns-cloud.net
    data:      Minimum TTL                   : 300
    data:      Refresh time                  : 900
    data:      Retry time                    : 300
    data:                                    :
<BR>
De NS-records die zijn gemaakt met de zone wilt weergeven, gebruikt u de volgende opdracht:

    azure network dns record-set show myresourcegroup "contoso.com" "@" NS
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/NS
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    NS records
    data:        Name server domain name     : ns1-05.azure-dns.com
    data:        Name server domain name     : ns2-05.azure-dns.net
    data:        Name server domain name     : ns3-05.azure-dns.org
    data:        Name server domain name     : ns4-05.azure-dns.info
    data:
    info:    network dns-record-set show command OK

>[AZURE.NOTE] Record wordt ingesteld op de hoofdmap (of *Top*) van een DNS-Zone gebruik **@** zoals de naam van de record hebt ingesteld.

## <a name="test"></a>Test

U kunt uw DNS-zone testen met behulp van DNS-hulpprogramma's zoals nslookup, GRAVEN, of de `Resolve-DnsName` PowerShell-cmdlet.

Als u uw domein voor het gebruik van de nieuwe zone in Azure DNS nog niet hebt overgedragen, moet u de DNS-query rechtstreeks naar een van de naamservers voor uw zone verwijzen. De naamservers voor uw zone staan vermeld in de NS-records, zoals aangegeven per "weergeven azure netwerk DNS-record instellen" hierboven. Moet de SUBSTITUEREN de juiste waarden voor de tijdzone in de onderstaande opdracht.

Het volgende voorbeeld wordt GRAVEN om query's in het domein contoso.com met de naamservers voor de DNS-zone toegewezen. De query heeft zodat deze verwijzen naar een naamserver waarvoor we gebruikt * @ * en naam van de zone met GRAVEN.

     <<>> DiG 9.10.2-P2 <<>> @ns1-05.azure-dns.com contoso.com
    (1 server found)
    global options: +cmd
    Got answer:
    ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60963
    flags: qr aa rd; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
    WARNING: recursion requested but not available

    OPT PSEUDOSECTION:
    EDNS: version: 0, flags:; udp: 4000
    QUESTION SECTION:
    contoso.com.                        IN      A

    AUTHORITY SECTION:
    contoso.com.         300     IN      SOA     edge1.azuredns-cloud.net.
    msnhst.microsoft.com. 6 900 300 604800 300

    Query time: 93 msec
    SERVER: 208.76.47.5#53(208.76.47.5)
    WHEN: Tue Jul 21 16:04:51 Pacific Daylight Time 2015
    MSG SIZE  rcvd: 120

## <a name="next-steps"></a>Volgende stappen

Na het maken van een DNS-zone, maken [Recordsets en records](dns-getstarted-create-recordset-cli.md) om te beginnen met het omzetten van namen voor uw domein Internet.
