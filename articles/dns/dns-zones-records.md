<properties 
   pageTitle="DNS-Zones en Records | Microsoft Azure" 
   description="Overzicht van ondersteuning voor het hosten van DNS-zones en records in Microsoft Azure DNS." 
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
   ms.date="10/17/2016"
   ms.author="jtuliani"/>

# <a name="dns-zones-and-records"></a>DNS-zones en records

Deze pagina wordt uitgelegd dat de belangrijkste concepten van domeinen, DNS-zones, en DNS-records en record sets en hoe deze worden ondersteund in Azure DNS.

## <a name="domain-names"></a>Domeinnamen
 
De Domain Name System is een hiërarchie van domeinen. De hiërarchie begint van het domein 'wortel', waarvan de naam gewoon is**.**.  Hieronder volgt afkomstig zijn op het hoogste niveau domeinen, zoals 'com', 'netto', 'organigram', 'uk' of 'jp'.  Zijn onder de domeinen van de tweede niveau, zoals 'org.uk' of 'co.jp'. De domeinen in de DNS-hiërarchie zijn globaal verdeeld, die worden gehost door DNS-naamservers overal ter wereld.

Een domeinnaamregistrar is een organisatie waarmee u een domeinnaam, zoals 'contoso.com' kopen.  Een domeinnaam kopen, kunt u de rechterkant om te bepalen de DNS-hiërarchie onder die naam, bijvoorbeeld zodat u kunt de naam 'www.contoso.com' naar de website van uw bedrijf verwijzen. De registrar mogelijk hosten het domein in een eigen naamservers namens u of u kunt ook alternatieve naamservers opgeven.

Azure DNS biedt een globaal verdeelde, hoge beschikbaarheid naam-serverinfrastructuur die u gebruiken kunt voor het hosten van uw domein. Door uw domeinen in Azure DNS host, kunt u uw DNS-records met dezelfde referenties, API's, hulpmiddelen, facturering, en ondersteuning beheren als uw andere Azure services.

Azure DNS ondersteunt geen momenteel kopen van domeinnamen. Als u een domeinnaam kopen wilt, moet u een derde partij domeinnaamregistrar gebruiken. De domeinregistrar, worden meestal kosten een kleine jaarlijkse. De domeinen kunnen vervolgens worden gehost in Azure DNS voor het beheer van DNS-records. Zie [gemachtigde een domein aan Azure DNS](dns-domain-delegation.md) voor meer informatie.

## <a name="dns-zones"></a>DNS-zones 

Een DNS-zone wordt gebruikt voor het hosten van de DNS-records voor een bepaald domein. Als u wilt hosten van uw domein in Azure DNS, moet u een DNS-zone voor die domeinnaam maken. Elke DNS-record voor uw domein wordt dan in deze DNS-zone gemaakt. 

Het domein 'contoso.com' kan bijvoorbeeld verschillende DNS-records, zoals 'mail.contoso.com' (voor een e-mailserver) en 'www.contoso.com' (voor een website) bevatten.

Wanneer u een DNS-zone maakt in Azure DNS, is de naam van de zone moet uniek zijn binnen de resourcegroep. Dezelfde zonenaam kan in een andere resource-groep of een ander abonnement van Azure worden gebruikt. Wanneer meerdere zones dezelfde naam delen, wordt elk exemplaar verschillende adressen van naamservers toegewezen. Slechts één set adressen kan worden geconfigureerd met de domeinnaamregistrar.

>[AZURE.NOTE] U hoeft niet te eigenaar bent van een domeinnaam maken van een DNS-zone met die domeinnaam in Azure DNS. Echter, hoeft u de eigenaar bent van het domein, zodat het configureren van de Azure DNS-naamservers als de juiste naamservers voor de naam van het domein met de domeinnaamregistrar.

Zie de [gemachtigde een domein aan Azure DNS](dns-domain-delegation.md)voor meer informatie.

## <a name="dns-records"></a>DNS-records

### <a name="record-types"></a>Recordtypen

Elke DNS-record heeft een naam en een type. Records worden ingedeeld in verschillende typen op basis van de gegevens bevatten. De meest voorkomende type is een 'A' record, die een naam wordt toegewezen aan een IPv4-adres. Een andere voorkomende type is een 'MX'-record, wijst een naam met een e-mailserver.

Azure DNS ondersteunt alle algemene DNS-recordtypen: A, AAAA, CNAME, MX, NS, PTR, SOA, SRV en TXT.

### <a name="record-names"></a>Record namen

Records zijn opgegeven in Azure DNS, met behulp van relatieve namen. Een *FQDN* (Fully Qualified domain name) bevat de zonenaam van de dat de naam van een *relatieve* niet betekent. De relatieve record naam 'www' in de zone 'contoso.com' biedt bijvoorbeeld de volledig gekwalificeerde naam van de 'www.contoso.com'.

Een *Top* -record is een DNS-record op de hoofdmap (of *Top*) van een DNS-zone. Bijvoorbeeld, in de DNS-zone 'contoso.com', een record Top, heeft ook de volledig gekwalificeerde naam contoso.com (dit is soms een *Open* domein genoemd).  Door de overeenkomst, de naam van de relatieve '@' wordt gebruikt om Top-records te maken.

### <a name="record-sets"></a>Record sets
Soms moet u meer dan één DNS-record maken met een opgegeven naam en type. Stel dat de website 'www.contoso.com' wordt gehost in twee verschillende IP-adressen. De website van twee verschillende een records, één voor elk IP-adres is vereist. Dit is een voorbeeld van een Recordset:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS beheert DNS-records met *record wordt ingesteld*. Een record set (ook wel bekend als een *resource* Recordset) is de verzameling DNS-records in een zone met dezelfde naam hebben zijn van hetzelfde type. De meeste record sets één record bevatten, maar voorbeelden zoals dit item, waarin een recordset meer dan één record bevat niet niet vaak voorkomen.

Bijvoorbeeld Stel dat u al een A-record 'www' hebt gemaakt in de zone 'contoso.com', '134.170.185.46' (de eerste record hierboven) aan te wijzen met de IP-adres.  Als u wilt maken van de tweede record zou u die record toevoegen aan de bestaande recordset, plaats maken van een nieuwe recordset.

De typen SOA- en CNAME-records zijn uitzonderingen. De DNS-standaarden niet toestaan dat meerdere records met dezelfde naam voor deze typen, dus deze record sets slechts één record kunnen bevatten.

### <a name="time-to-live"></a>Time to live

De TTL of TTL Hiermee wordt opgegeven hoe lang elke record is opgeslagen in de cache door clients voordat het opnieuw in de query wordt uitgevoerd. Klik in het bovenstaande voorbeeld is de TTL-waarde 3600 seconden of 1 uur staan.

De TTL is opgegeven voor de recordset, niet voor elke record, in Azure DNS, zodat dezelfde waarde wordt gebruikt voor alle records in die recordset.  U kunt elke TTL-waarde tussen 1 en 2.147.483.647 seconden opgeeft.

### <a name="wildcard-records"></a>Jokertekens records

Azure DNS ondersteunt [jokertekens records](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Jokertekens records worden geretourneerd in antwoord op een query met de naam van een overeenkomende (tenzij er meer geschikt is van een niet-jokertekens Recordset). Jokertekens opnemen sets worden ondersteund voor alle recordtypen behalve NS en SOA.  

Als u wilt maken in een recordset jokertekens, gebruikt u de naam van de recordset '\*'. U kunt ook ook gebruiken een naam met '\*'als de meest linkse label, bijvoorbeeld'\*.foo'.

### <a name="cname-records"></a>CNAME-records

CNAME-record sets kunnen niet worden gecombineerd met andere records sets met dezelfde naam. U kan bijvoorbeeld een CNAME-record instellen met de naam van de relatieve 'www' en een A-record maken met de naam van de relatieve 'www' op hetzelfde moment.

Omdat de zone Top (naam = ‘@’) bevat altijd de NS en SOA opnemen sets die zijn gemaakt wanneer de zone is gemaakt, kunt u een CNAME-record instellen op de top zone geen maken.

Deze beperkingen voortvloeien uit de DNS-standaarden en zijn geen beperkingen van Azure DNS.

### <a name="ns-records"></a>NS-records

Een recordset NS wordt automatisch gemaakt op de top van elke zone (naam = ‘@’), en automatisch wordt verwijderd wanneer de zone wordt verwijderd (deze niet worden verwijderd afzonderlijk).  U kunt de TTL-waarde van deze recordset wijzigen, maar u kunt de records die zijn vooraf geconfigureerde om te verwijzen naar de DNS-Azure-naamservers die zijn toegewezen aan de zone niet wijzigen.

U kunt maken en verwijderen van andere NS-records in de zone, niet op de top zone.  Hiermee kunt u voor het configureren van de onderliggende zones (Zie [subdomeinen in Azure DNS overdragen](dns-domain-delegation.md#delegating-sub-domains-in-azure-dns).)

### <a name="soa-records"></a>SOA-records

Een recordset SOA wordt automatisch gemaakt op de top van elke zone (naam = ‘@’), en automatisch wordt verwijderd wanneer de zone wordt verwijderd.  SOA records kunnen niet worden gemaakt of afzonderlijk verwijderd. 

U kunt alle eigenschappen van de SOA-record, behalve voor de eigenschap 'host', die vooraf is geconfigureerd om te verwijzen naar de naam van de naam van de primaire-server verstrekt door Azure DNS wijzigen.

### <a name="spf-records"></a>SPF-records

Afzender beleid Framework (SPF)-records worden gebruikt om aan te geven welke e-mailservers u e-mail verzendt namens een opgegeven domeinnaam zijn toegestaan.  Juiste configuratie van de SPF-records is belangrijk om te voorkomen dat geadresseerden markering van uw e-mailen als 'ongewenste e-mail'.

De DNS-RFC's geïntroduceerd oorspronkelijk een nieuwe 'SPF' recordtype voor dit scenario. Ter ondersteuning van oudere naamservers, toegestaan ze ook het gebruik van het recordtype TXT om op te geven van de SPF-records.  Deze dubbelzinnigheden tot verwarring, die is opgelost met [RFC 7208](http://tools.ietf.org/html/rfc7208#section-3.1)hebben geleid.  Dit vermeld dat SPF-records moeten alleen worden gemaakt met het recordtype TXT en de SPF-recordtype afgeschaft. 

**SPF-records worden ondersteund door Azure DNS en moeten worden gemaakt met behulp van het recordtype TXT.** Het verouderde SPF-recordtype wordt niet ondersteund. Wanneer het [bestand importeren van een DNS-zone](dns-import-export.md), een SPF-records met het type van de SPF-record worden geconverteerd naar het recordtype TXT. 

### <a name="srv-records"></a>SRV-records

[SRV-records](https://en.wikipedia.org/wiki/SRV_record) worden gebruikt door de verschillende services serverlocaties op te geven. Wanneer u een SRV-record opgeeft in Azure DNS:

- De *service* en *protocol* moeten worden opgegeven als onderdeel van de naam van de recordset voorafgegaan door onderstrepingstekens.  Bijvoorbeeld '\_sip. \_tcp.name'.  Voor een record op de zone-Top, is niet nodig om op te geven '@' in de recordnaam van de gebruiken de service en protocol, bijvoorbeeld '\_sip. \_tcp'.
- De *prioriteit*, *gewicht*, *poorten*en *doel* zijn opgegeven als parameters van elke record in de recordset. 

## <a name="tags-and-metadata"></a>Labels en metagegevens

### <a name="tags"></a>Labels
Labels zijn van een lijst met naam-waardeparen en door Azure Resource Manager label resources worden gebruikt.  Azure resourcemanager labels gebruikt gefilterde weergaven van uw factuur Azure inschakelen en ook kunt u instellen van een beleid waarop labels vereist zijn. Zie voor meer informatie over de labels, [werken met tags ordenen van uw Azure resources](../resource-group-using-tags.md).

Azure DNS ondersteunt resourcemanager Azure-labels gebruiken op DNS-zone resources.  Labels worden niet ondersteund op DNS-record sets.

### <a name="metadata"></a>Metagegevens

Als alternatief voor labels recordset ondersteunt Azure DNS record sets met 'metagegevens' aantekeningen te maken.  Metagegevens is vergelijkbaar met labels, kunt u naam-waardeparen koppelen aan elke recordset.  Dit is handig, bijvoorbeeld tot record het doel van elke recordset.  In tegenstelling tot tags metagegevens kan niet worden gebruikt om een gefilterde weergave van uw Azure factuur en kan niet worden opgegeven in een resourcemanager Azure-beleid.

## <a name="limits"></a>Limieten

De volgende standaardlimieten toepassen wanneer met Azure DNS:

[AZURE.INCLUDE [dns-limits](../../includes/dns-limits.md)] 

## <a name="next-steps"></a>Volgende stappen

- Als u wilt beginnen met Azure DNS, Leer hoe u [maakt een DNS-zone](dns-getstarted-create-dnszone-portal.md) en [DNS-records maken](dns-getstarted-create-recordset-portal.md).
- Als u wilt migreren van een bestaande DNS-zone, Leer hoe u [importeren en DNS zone file](dns-import-export.md).
