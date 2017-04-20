#### <a name="create-an-a-record-set-with-single-record"></a>Maak een A-record instellen met één record

Als u wilt maken in een recordset, gebruikt u `azure network dns record-set create`. Geef de zonenaam van de, in de resourcegroep record relatieve naam, recordtype en tijd aan live (TTL) instellen.

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300

Na het maken van de A record instellen, het IPv4-adres toevoegen aan de record die is geconfigureerd met `azure network dns record-set add-record`.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1

#### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Maak een AAAA-record instellen met een enkele record

    azure network dns record-set create myresourcegroup contoso.com "test-aaaa" AAAA --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-aaaa" AAAA -b "2607:f8b0:4009:1803::1005"

#### <a name="create-a-cname-record-set-with-a-single-record"></a>Maak een CNAME-record instellen met een enkele record

CNAME-records kunnen alleen één enkele tekenreekswaarde.


    azure network dns record-set create -g myresourcegroup contoso.com  "test-cname" CNAME --ttl 300

    azure network dns record-set add-record  myresourcegroup contoso.com  test-cname CNAME -c "www.contoso.com"


#### <a name="create-an-mx-record-set-with-a-single-record"></a>Maak een MX-record instellen met een enkele record

In dit voorbeeld gebruiken we de naam van de recordset "@" de MX-record maken op de zone Top (in dit geval "contoso.com"). Dit geldt voor MX-records.

    azure network dns record-set create myresourcegroup contoso.com  "@"  MX --ttl 300

    azure network dns record-set add-record -g myresourcegroup contoso.com  "@" MX -e "mail.contoso.com" -f 5


#### <a name="create-an-ns-record-set-with-a-single-record"></a>Maak een NS-record instellen met een enkele record

    azure network dns record-set create myresourcegroup contoso.com test-ns  NS --ttl 300

    azure network dns record-set add-record myresourcegroup  contoso.com  "test-ns" NS -d "ns1.contoso.com"

#### <a name="create-a-ptr-record-set-with-a-single-record"></a>Maak een PTR-record instellen met een enkele record  
In dit geval ' Mijn-volgens-zone.com' volgens zones waarmee uw IP-bereik vertegenwoordigt.  Elke PTR-record instellen in deze zone overeenkomt met een IP-adres in deze IP-bereik.    

    azure network dns record-set add-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"   

#### <a name="create-an-srv-record-set-with-a-single-record"></a>Een SRV-record instellen met één record maken

Als u een SRV-record in de hoofdmap van de zone maakt, kunt u '_service' en '_protocol' in de recordnaam van de. Er is niet nodig om "@" in de recordnaam van de.


    azure network dns record-set create myresourcegroup contoso.com "_sip._tls" SRV --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 - w 5 -o 8080 -u "sip.contoso.com"

#### <a name="create-a-txt-record-set-with-single-record"></a>Een TXT-record instellen met één record maken

    azure network dns record-set create myresourcegroup contoso.com "test-TXT" TXT --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-txt" TXT -x "this is a TXT record"
