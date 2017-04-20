## <a name="about-records"></a>Over records

Elke DNS-record heeft een naam en een type. Records worden ingedeeld in verschillende typen op basis van de gegevens bevatten. De meest voorkomende type is een 'Een'-record, die een naam wordt toegewezen aan een IPv4-adres. Een ander type is een "MX"-record, wijst een naam met een e-mailserver.

Azure DNS ondersteunt alle algemene DNS-recordtypen, waaronder A, AAAA, CNAME, MX, NS, PTR, SOA, SRV en TXT. Houd rekening met:
- SOA-record sets worden automatisch gemaakt met elke zone, ze afzonderlijk kunnen niet worden gemaakt.
- SPF-records moeten worden gemaakt met behulp van het recordtype TXT. Zie [deze pagina](http://tools.ietf.org/html/rfc7208#section-3.1)voor meer informatie.

Records zijn opgegeven in Azure DNS, met behulp van relatieve namen. Een "" FQDN-naam (FQDN) bevat de zonenaam van de dat de naam van een "relatieve" niet betekent. De relatieve record naam 'www' in de zone 'contoso.com' biedt bijvoorbeeld de volledig gekwalificeerde naam van de record www.contoso.com.

## <a name="about-record-sets"></a>Over record sets

Soms moet u meer dan één DNS-record maken met een opgegeven naam en type. Stel dat de website "www.contoso.com" wordt gehost in twee verschillende IP-adressen. De website van twee verschillende een records, één voor elk IP-adres is vereist. Dit is een voorbeeld van een Recordset:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS beheert DNS-records met behulp van de record sets. Een recordset is de verzameling DNS-records in een zone met dezelfde naam hebben zijn hetzelfde type. De meeste record sets één record bevatten, maar voorbeelden zoals dit item, waarin een recordset meer dan één record bevat niet niet vaak voorkomen.

SOA- en CNAME-record sets zijn uitzonderingen. De DNS-standaarden niet toestaan dat meerdere records met dezelfde naam voor deze typen.

De TTL of TTL Hiermee wordt opgegeven hoe lang elke record is opgeslagen in de cache door clients voordat het opnieuw in de query wordt uitgevoerd. In dit voorbeeld wordt de TTL is 3600 seconden of 1 uur staan. De TTL is opgegeven voor de recordset, niet voor elke record, zodat u dezelfde waarde wordt gebruikt voor alle records in die recordset.

#### <a name="wildcard-record-sets"></a>Jokertekens recordsets

Azure DNS ondersteunt [jokertekens records](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Deze worden geretourneerd voor een query met de naam van een overeenkomende (tenzij er meer geschikt is van een niet-jokertekens Recordset). Jokertekens opnemen sets worden ondersteund voor alle recordtypen behalve NS en SOA.  

U kunt een recordset jokertekens maken met de naam van de recordset "\*". Of, gebruikt u een naam met het label '\*", bijvoorbeeld"\*.foo ".

#### <a name="cname-record-sets"></a>CNAME-record sets

CNAME-record sets kunnen niet worden gecombineerd met andere records sets met dezelfde naam. U kan bijvoorbeeld een CNAME-record instellen met de naam van de relatieve 'www' en een A-record maken met de naam van de relatieve 'www' op hetzelfde moment. Omdat de zone Top (naam = ‘@’) bevat altijd de NS en SOA opnemen sets die zijn gemaakt wanneer de zone is gemaakt, kunt u een CNAME-record instellen op de top zone geen maken. Deze beperkingen voortvloeien uit de DNS-standaarden en beperkingen van Azure DNS niet.
