<properties
   pageTitle="Aan de slag met Azure DNS | Microsoft Azure"
   description="Informatie over het maken van DNS-zones voor Azure DNS. Dit is een stap voor stap om uw eerste DNS-zone gemaakt om te beginnen met het hosten van uw DNS-domein via PowerShell."
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

# <a name="create-a-dns-zone-using-powershell"></a>Een DNS-zone via Powershell maken

> [AZURE.SELECTOR]
- [Azure-Portal](dns-getstarted-create-dnszone-portal.md)
- [PowerShell](dns-getstarted-create-dnszone.md)
- [Azure CLI](dns-getstarted-create-dnszone-cli.md)

In dit artikel begeleidt u door de stappen voor het maken van een DNS-zone via PowerShell. U kunt ook een DNS-zone met CLI of de Azure-portal maken.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="tagetag"></a>Over Etags en labels

### <a name="etags"></a>Etags

Stel dat twee personen of twee processen wil een DNS-record op hetzelfde moment wijzigen. Welke wins? En de winnaar weet dat ze alleen de wijzigingen die zijn gemaakt door iemand anders hebt overschreven?

Azure DNS gebruikt Etags worden afgehandeld gelijktijdige wijzigingen in dezelfde resource veilig. Elke resource DNS (zone of recordset) heeft een Etag gekoppeld. Wanneer een resource zijn opgehaald, zijn er ook de Etag opgehaald. Wanneer een resource wordt bijgewerkt, hebt u de optie voor het doorgeven weer de Etag zodat Azure DNS kunt controleren of de Etag op de server overeenkomsten. Aangezien elke update aan een resourceweergave in de Etag opnieuw worden gegenereerd resulteert, wordt een verkeerde Etag combinaties aangegeven dat een gelijktijdige wijziging is doorgevoerd. Etags worden ook gebruikt bij het maken van een nieuwe resource om ervoor te zorgen dat de resource nog niet bestaat.

Azure DNS PowerShell standaard Etags blokkeren gelijktijdige wijzigingen in zones en sets opnemen. Het optionele *-overschrijven* veranderen kunnen worden gebruikt om Etag controles onderdrukken, in dat geval moeten gelijktijdige wijzigingen die zijn opgetreden worden overschreven.

Op het niveau van de Azure DNS REST API, zijn Etags opgegeven met HTTP-headers.  Het gedrag is opgenomen in de volgende tabel:

|Koptekst|Gedrag|
|------|--------|
|Geen|OPSLAG altijd is uitgevoerd (geen Etag controles)|
|If-overeenkomst<etag>|OPSLAG slaagt alleen als resource bestaat en Etag overeenkomt met|
|If-match *     | OPSLAG slaagt alleen als resource bestaat|
|If-none-match * |  OPSLAG slaagt alleen als resource niet bestaat|

### <a name="tags"></a>Labels

Markeringen zijn anders dan Etags. Labels zijn van een lijst met naam-waardeparen en door Azure Resource Manager naar label bronnen voor factuur- of groepeerniveau worden gebruikt. Zie voor meer informatie over de labels, [werken met tags ordenen van uw Azure resources](../resource-group-using-tags.md).

Azure DNS PowerShell ondersteunt Tags zowel zones als record sets opgegeven met de opties op `-Tag` parameter.


## <a name="before-you-begin"></a>Voordat u begint

Controleer of u de volgende items voordat u begint met de configuratie.

- Een Azure-abonnement. Als u nog geen een Azure-abonnement hebt, kunt u uw [MSDN abonnee voordelen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of Registreer omhoog activeren voor een [gratis-account](https://azure.microsoft.com/pricing/free-trial/).

- U moet de nieuwste versie van de Azure resourcemanager PowerShell-cmdlets (1,0 of later) installeren. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor meer informatie over het installeren van de PowerShell-cmdlets.

## <a name="step-1---sign-in"></a>Stap 1: aanmelden

Open uw PowerShell-console en verbinding maken met uw account. Zie [Windows PowerShell gebruiken met Resource Manager](../powershell-azure-resource-manager.md)voor meer informatie.

Gebruik in het onderstaande voorbeeld om te verbinden:

    Login-AzureRmAccount

Controleer de abonnementen voor het account.

    Get-AzureRmSubscription

Geef het abonnement waaraan u wilt gebruiken.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="step-2---create-a-resource-group"></a>Stap 2: een resourcegroep maken

Azure resourcemanager is vereist dat alle resourcegroepen Geef een locatie. Dit wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Echter omdat alle DNS-resources globale, niet regionale zijn, heeft de keuze van resource groep locatie geen invloed op Azure DNS.

Als u een bestaande resourcegroep gebruikt, kunt u deze stap overslaan.

    New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"


## <a name="step-3---register"></a>Stap 3: Register

De service Azure DNS wordt beheerd door de provider van de resource Microsoft.Network. Uw abonnement op Azure moet worden geregistreerd voor het gebruik van deze resource-provider voordat u Azure DNS kunt gebruiken. Dit is een eenmalige bewerking voor elk abonnement.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network


## <a name="step-4----create-a-dns-zone"></a>Stap 4: een DNS-zone maken

Een DNS-zone wordt gemaakt met de `New-AzureRmDnsZone` cmdlet. Er zijn voorbeelden hieronder voor het maken van een DNS-zone, met of zonder markeringen. Zie het gedeelte van [labels](#tags) in dit artikel voor meer informatie over de labels.

>[AZURE.NOTE] In Azure DNS zonenamen moeten worden opgegeven zonder een beëindiging **'.'**. Bijvoorbeeld als '**contoso.com**' plaats '**contoso.com.**'.

### <a name="to-create-a-dns-zone"></a>Een DNS-zone maken

Het volgende voorbeeld wordt gemaakt van een DNS-zone *contoso.com* genoemd in de resourcegroep *MyResourceGroup*genoemd. Het voorbeeld gebruiken om een DNS-zone, wordt vervangen door de waarden voor uw eigen te maken.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-create-a-dns-zone-with-tags"></a>Een DNS-zone maken met markeringen

Het volgende voorbeeld ziet u hoe een DNS-zone met twee labels, maakt *project demo =* en *Envelop = test*. Het voorbeeld gebruiken om een DNS-zone, wordt vervangen door de waarden voor uw eigen te maken.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @( @{ Name="project"; Value="demo" }, @{ Name="env"; Value="test" } )

## <a name="view-records"></a>Records weergeven

De volgende DNS-records maken van een DNS-zone ook worden gemaakt:

- De record *Start of Authority* (Start of). Dit is aanwezig zijn in de hoofdmap van elke DNS-zone.

- De naamserver serverrecords (Domeinnaamserver). Deze weergeven in welke naamservers host de zone. Azure DNS maakt gebruik van een groep naamservers en dus andere naamservers kunnen zijn aangewezen om verschillende zones in Azure DNS. Zie [een domein aan Azure DNS delegeren](dns-domain-delegation.md) voor meer informatie.

Gebruik deze records wilt weergeven, `Get-AzureRmDnsRecordSet`:

    Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 2b855de1-5c7e-4038-bfff-3a9e55b49caf
    RecordType        : SOA
    Records           : {[ns1-01.azure-dns.com,msnhst.microsoft.com,900,300,604800,300]}
    Tags              : {}

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 5fe92e48-cc76-4912-a78c-7652d362ca18
    RecordType        : NS
    Records           : {ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org,
                  ns4-01.azure-dns.info}
    Tags              : {}


Record wordt ingesteld op de hoofdmap (of *Top*) van een DNS-Zone gebruik **@** zoals de naam van de record hebt ingesteld.


## <a name="test"></a>Test

U kunt uw DNS-zone testen met behulp van DNS-hulpprogramma's zoals nslookup, graven of de [DNS-oplossen-naam PowerShell-cmdlet](https://technet.microsoft.com/library/jj590781.aspx).

Als u uw domein voor het gebruik van de nieuwe zone in Azure DNS nog niet hebt overgedragen, moet u de DNS-query rechtstreeks naar een van de naamservers voor uw zone verwijzen. De naamservers voor uw zone staan vermeld in de NS-records, zoals aangegeven per `Get-AzureRmDnsRecordSet` hierboven. Moet de SUBSTITUEREN de juiste waarden voor uw tijdzone in de onderstaande opdracht.

    nslookup
    > set type=SOA
    > server ns1-01.azure-dns.com
    > contoso.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    contoso.com
            primary name server = ns1-01.azure-dns.com
            responsible mail addr = msnhst.microsoft.com
            serial  = 1
            refresh = 900 (15 mins)
            retry   = 300 (5 mins)
            expire  = 604800 (7 days)
            default TTL = 300 (5 mins)


## <a name="next-steps"></a>Volgende stappen

Na het maken van een DNS-zone, maken [Recordsets en records](dns-getstarted-create-recordset.md) om te beginnen met het omzetten van namen voor uw domein Internet.

