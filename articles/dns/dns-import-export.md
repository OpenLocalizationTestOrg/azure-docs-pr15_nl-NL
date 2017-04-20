<properties
   pageTitle="Importeren en exporteren van een domein zonebestand naar Azure DNS CLI met | Microsoft Azure"
   description="Meer informatie over het importeren en exporteren van een DNS-zonebestand naar Azure DNS met behulp van Azure CLI"
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

# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli"></a>Importeren en exporteren van een DNS-zone-bestand met de CLI Azure


In dit artikel wordt u begeleid bij het importeren en exporteren van DNS-zone-bestanden voor gebruik van de Azure CLI Azure-DNS.

## <a name="introduction-to-dns-zone-migration"></a>Inleiding tot de DNS-zone migratie

Een DNS-zonebestand is een tekstbestand met details van elke record Domain Name System (DNS) in de zone. Een standaardindeling, zodat u geschikt is voor het overbrengen van DNS-records tussen de DNS-systemen volgt. Een zonebestand is een snelle, betrouwbare en handige manier om over te brengen van een DNS-zone, in- of uitzoomen op Azure DNS.

Azure DNS ondersteunt importeren en exporteren van zonebestanden met behulp van de Azure opdrachtregel (CLI). De CLI Azure is een platforms opdrachtregel hulpprogramma voor het beheren van Azure services. Dit is beschikbaar voor de platforms voor Windows, Mac en Linux vanaf de [pagina Azure downloads](https://azure.microsoft.com/downloads/). Ondersteuning voor meerdere platforms is vooral voor importeren en exporteren van zonebestanden, omdat de meest voorkomende naam serversoftware, [BINDEN](https://www.isc.org/downloads/bind/), meestal wordt uitgevoerd op Linux.

## <a name="obtain-your-existing-dns-zone-file"></a>Uw bestaande DNS-zonebestand verkrijgen

Voordat u een DNS-zonebestand in Azure DNS importeert, moet u een kopie van het zonebestand verkrijgt. De bron van dit bestand afhankelijk van waar de DNS-zone momenteel wordt gehost.

- Als uw DNS-zone wordt gehost door een partner-service (zoals een van de domeinregistrar, speciale DNS-hostingprovider of alternatieve cloud-provider), dient deze service de mogelijkheid om de DNS-zone-bestand te downloaden.

- Als uw DNS-zone wordt gehost op Windows-DNS, is de standaardmap voor de zonebestanden **%systemroot%\system32\dns**. Het volledige pad naar elke zonebestand wordt ook weergegeven op het tabblad **Algemeen** van de DNS service management console.

- Als uw DNS-zone wordt gehost via binding, wordt de locatie van het zonebestand voor elke zone is opgegeven in de binding configuratie bestand **named.conf**.

**Werken met zonebestanden bij GoDaddy**<BR>
Zonebestanden die zijn gedownload van GoDaddy hebben een iets niet-indeling. Moet u kunt deze voordat u deze zonebestanden in Azure DNS importeren. DNS-namen in de RData van elk van de DNS-records zijn opgegeven als volledig gekwalificeerde namen, maar ze niet hebben een beëindigen "." Dit betekent dat ze worden geïnterpreteerd door andere DNS-systemen als relatieve namen. U moet het zonebestand om toe te voegen de afsluitende bewerken '. ' naar hun namen voordat u ze in Azure DNS importeert.

## <a name="import-a-dns-zone-file-into-azure-dns"></a>Een DNS-zone-bestand importeren in Azure-DNS


Het importeren van een zonebestand maakt een nieuwe zone in Azure DNS als er nog niet bestaat. Als de zone al bestaat, moeten de record sets in het zonebestand worden samengevoegd met de bestaande record groepen.

### <a name="merge-behavior"></a>Gedrag samenvoegen

- Standaard worden samengevoegd in bestaande als nieuwe record sets. Identieke records in een samengevoegde Recordset zijn opgeheven gedupliceerde.

- U kunt ook door het opgeven van de `--force` de optie het importproces, vervangt bestaande record sets met de nieuwe record sets. Bestaande recordsets die ik heb een overeenkomende record instellen in het geïmporteerde zonebestand worden niet verwijderd.

- Als record sets worden samengevoegd, wordt de optie time to live (TTL) van de bestaande record sets gebruikt. Wanneer `--force` wordt gebruikt, de TTL-waarde van de nieuwe recordset wordt gebruikt.

- Begin met parameters die instantie (SOA) (behalve `host`) altijd worden opgehaald uit het geïmporteerde zonebestand, ongeacht of u `--force` wordt gebruikt. Op dezelfde manier voor de naamserver-record instellen op de zone-Top, is de TTL altijd overgenomen uit het geïmporteerde zonebestand.

- Een geïmporteerde CNAME-record niet een bestaande CNAME-record met dezelfde naam vervangen tenzij de `--force` parameter is opgegeven.

- Als een conflict optreedt tussen een CNAME-record en een andere record met dezelfde naam maar met een ander type (ongeacht welke is bestaande of nieuwe), wordt de bestaande record bewaard. Dit is afhankelijk van het gebruik van `--force`.

### <a name="additional-information-about-importing"></a>Als u meer informatie over het importeren

De volgende notities bieden importproces voor aanvullende technische details over de zone.

- De `$TTL` richtlijn is optioneel, en wordt ondersteund. Als er geen `$TTL` richtlijn wordt uitgedrukt, records zonder een expliciete TTL geïmporteerde instellen op een standaard-TTL van 3600 seconden. Wanneer twee records in de dezelfde recordset verschillende TTLs opgeeft, wordt de lagere waarde wordt gebruikt.

- De `$ORIGIN` richtlijn is optioneel, en wordt ondersteund. Als er geen `$ORIGIN` is ingesteld, de standaardwaarde gebruikt is de zonenaam die is opgegeven op de opdrachtregel (plus de afsluitende ".").

- De `$INCLUDE` en `$GENERATE` richtlijnen worden niet ondersteund.

- Deze recordtypen worden ondersteund: A, AAAA, CNAME, MX, NS, SOA, SRV en TXT.

- De SOA-record wordt automatisch gemaakt door Azure DNS wanneer een zone wordt gemaakt. Als u een zonebestand importeren, alle SOA parameters overgenomen van de zone-bestand *, behalve* de `host` parameter. Deze parameter gebruikt de waarde die is verstrekt door Azure DNS. Dit is omdat deze parameter naar de naamserver van de primaire van Azure DNS verwijzen moet.

- De naamserver-record instellen op de top zone wordt ook automatisch gemaakt door Azure DNS wanneer de zone wordt gemaakt. Alleen met de TTL-waarde van deze recordset is geïmporteerd. Deze records bevatten de servernamen naam is verstrekt door Azure DNS. De gegevens van de record is niet overschreven door de waarden in het geïmporteerde zonebestand.

- Azure DNS ondersteunt in openbare voorbeeldweergave, slechts één tekenreeks TXT records. Multistring TXT-records worden samengevoegd en afgekapt tot 255 tekens.

### <a name="cli-format-and-values"></a>CLI-indeling en waarden


De opmaak van de opdracht Azure CLI om het importeren van een DNS-zone luidt als volgt:<BR>`azure network dns zone import [options] <resource group> <zone name> <zone file name>`

Waarden:

- `<resource group>`is de naam van de resourcegroep voor de zone in Azure DNS.
- `<zone name>`is de naam van de zone.
- `<zone file name>`is het pad/naam van de te importeren zonebestand.

Als een zone met deze naam niet in de resourcegroep bestaat, wordt het wordt gemaakt voor u. Als de zone al bestaat, wordt de geïmporteerde record sets met bestaande record sets worden samengevoegd. Als u wilt overschrijven de bestaande record groepen, gebruikt u de `--force` optie.

Als u wilt controleren of de opmaak van een zonebestand zonder daadwerkelijk te importeren, gebruikt u de `--parse-only` optie.

### <a name="step-1-import-a-zone-file"></a>Stap 1. Een zonebestand importeren

Een zonebestand voor de zone **contoso.com**importeren.

1. Meld u aan bij uw Azure-abonnement met behulp van de Azure CLI.

        azure login

2. Selecteer het abonnement waarop u wilt maken van uw nieuwe DNS-zone.

        azure account set <subscription name>

3. Azure DNS is een Azure resourcemanager alleen-lezen-service, zodat de CLI Azure resourcemanager modus moet worden veranderd.

        azure config mode arm

4. Voordat u de DNS-Azure-service gebruiken, moet u uw abonnement als u wilt gebruiken, de provider van de resource Microsoft.Network registreren. (Dit is een eenmalige bewerking voor elk abonnement).

        azure provider register Microsoft.Network

5. Als u niet een al hebt, moet u ook een resourcegroep resourcemanager maken.

        azure group create myresourcegroup westeurope

6. Als u wilt de zone **contoso.com** uit het bestand **contoso.com.txt** importeren in een nieuwe DNS-zone, in de resource groep **myresourcegroup**, voert u de opdracht `azure network dns zone import`.<BR>Deze opdracht wordt het zonebestand laden en parseren op. De opdracht wordt een reeks opdrachten worden uitgevoerd op de DNS-Azure-service om de zone te maken en alle van de record Hiermee stelt u in de zone. De opdracht wordt ook rapporteren in het consolevenster samen met eventuele fouten of waarschuwingen. Omdat record sets worden gemaakt in reeks, wordt het duurt een paar minuten een grote zone-bestand importeren.

        azure network dns zone import myresourcegroup contoso.com contoso.com.txt



### <a name="step-2-verify-the-zone"></a>Stap 2. Controleer of de zone

Als u wilt controleren of de DNS-zone nadat u het bestand hebt geïmporteerd, kunt u een van de volgende methoden:

- U kunt de records weergeven met behulp van de volgende opdracht uit Azure CLI.

        azure network dns record-set list myresourcegroup contoso.com

- U kunt de records weergeven met behulp van de PowerShell-cmdlet `Get-AzureRmDnsRecordSet`.

- U kunt `nslookup` om te controleren of naamresolutie voor de records. Omdat de zone nog niet is overgedragen, moet u de juiste Azure DNS-naamservers expliciet opgeven. Het onderstaande voorbeeld ziet hoe u de namen van de naam-server die zijn toegewezen aan de zone ophalen. IT ook ziet u hoe u de record 'www' query met behulp van `nslookup`.

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up the DNS Record Set "@" of type "NS"
        data:Id: /subscriptions/…/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
        data:Name: @
        data:Type: Microsoft.Network/dnszones/NS
        data:Location: global
        data:TTL : 3600
        data:NS records
        data:Name server domain name : ns1-01.azure-dns.com
        data:Name server domain name : ns2-01.azure-dns.net
        data:Name server domain name : ns3-01.azure-dns.org
        data:Name server domain name : ns4-01.azure-dns.info
        data:
        info:network dns record-set show command OK

        C:\> nslookup www.contoso.com ns1-01.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221

### <a name="step-3-update-dns-delegation"></a>Stap 3. DNS-delegering bijwerken

Nadat u hebt gecontroleerd dat de zone correct is geïmporteerd, moet u bij de DNS-delegering zodat deze verwijzen naar de Azure DNS-naamservers. Zie het artikel [de DNS-delegering bijwerken](dns-domain-delegation.md)voor meer informatie.

## <a name="export-a-dns-zone-file-from-azure-dns"></a>Een DNS-zonebestand exporteren vanuit Azure-DNS

De opmaak van de opdracht Azure CLI om het importeren van een DNS-zone luidt als volgt:<BR>`azure network dns zone export [options] <resource group> <zone name> <zone file name>`

Waarden:

- `<resource group>`is de naam van de resourcegroep voor de zone in Azure DNS.
- `<zone name>`is de naam van de zone.
- `<zone file name>`is het pad van het bronbestand zone worden geëxporteerd.

Als met de importbewerking zone nodig u eerst aan te melden, kiest u uw abonnement en configureren van de Azure CLI resourcemanager modus.

### <a name="to-export-a-zone-file"></a>Een zonebestand exporteren


1. Meld u aan bij uw Azure-abonnement met behulp van de Azure CLI.

        azure login

2. Selecteer het abonnement waarop u wilt maken van uw nieuwe DNS-zone.

        azure account set <subscription name>

3. Azure DNS is een Azure resourcemanager alleen-lezen-service. De CLI Azure moet worden veranderd naar resourcemanager-modus.

        azure config mode arm

4. Als u wilt exporteren de bestaande Azure DNS-zone **contoso.com** in resource groep **myresourcegroup** naar het bestand **contoso.com.txt** (in de huidige map), worden uitgevoerd `azure network dns zone export`. Deze opdracht wordt de service Azure DNS-recordsets in de zone opsommen en de resultaten exporteren naar een zonebestand binding-compatibele bellen.

        azure network dns zone export myresourcegroup contoso.com contoso.com.txt

