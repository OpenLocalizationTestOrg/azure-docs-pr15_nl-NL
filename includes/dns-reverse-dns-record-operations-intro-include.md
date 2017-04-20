## <a name="what-is-reverse-dns"></a>Wat is omgekeerde DNS?

Gebruikelijke DNS-records voor het inschakelen van een toewijzing van een DNS-naam (zoals 'www.contoso.com') aan een IP-adres (zoals 64.4.6.100).  Omgekeerde DNS zorgt ervoor dat de vertaling van een IP-adres (64.4.6.100) terug naar een naam (www.contoso.com).

Omgekeerde DNS-records worden in verschillende situaties gebruikt. Omgekeerde DNS-records zijn bijvoorbeeld veel gebruikt in de strijd tegen ongewenste e-mail door te verifiëren van de afzender van een e-mailbericht.  De ontvangst e-mailserver wordt opgehaald van de omgekeerde DNS-record van het verzendende server IP-adres en controleer of als host is geautoriseerd om e-mail te verzenden vanaf het oorspronkelijke domein. (Houd er rekening mee echter die [services Azure berekenen verzendende e-mailberichten naar externe domeinen niet ondersteunen](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).)

## <a name="how-reverse-dns-works"></a>Hoe omgekeerde DNS werkt

Omgekeerde DNS-records zijn ingesloten in een speciale DNS-zones, bekend als 'Volgens' zones.  Deze zones vormen een afzonderlijke DNS-hiërarchie in combinatie met de normale hiërarchie hostingprovider domeinen zoals 'contoso.com'.

De DNS-record 'www.contoso.com' is bijvoorbeeld geïmplementeerd met een "A" DNS-record met de naam 'www' in de zone contoso.com.  Deze A-record verwijst naar het bijbehorende IP-adres, in dit geval 64.4.6.100.  De reverse lookup wordt geïmplementeerd afzonderlijk, met een 'PTR'-record met de naam '100' in de zone '6.4.64.in-addr.arpa' (Let op dat IP-adressen in volgens zones worden omgekeerd.)  Deze PTR-record, verwijst als het goed is geconfigureerd naar de naam www.contoso.com.

Wanneer een organisatie is een geblokkeerde IP-adres is toegewezen, aanschaffen ze ook rechts voor het beheren van de corresponderende volgens zone. De volgens zones overeenkomt met het IP-adresblokken die worden gebruikt door Azure worden gehost en beheerd door Microsoft. Uw Internetprovider mogelijk de zone volgens voor uw eigen IP-adressen voor u, of er als gastheer mogelijk kunt u de zone volgens een DNS-service van uw keuze, zoals Azure DNS gehost.

>[AZURE.NOTE] Doorsturen DNS-lookups en omgekeerde DNS-lookups zijn geïmplementeerd in afzonderlijke, parallelle DNS-hiërarchieën. Het omgekeerde zoekactie voor 'www.contoso.com' is **niet** wordt gehost in de zone 'contoso.com', liever die wordt gehost in de zone volgens voor het bijbehorende IP-Adresblok.

Voor meer informatie over het omgekeerde DNS, raadpleegt u [Omgekeerde DNS-zoekopdracht](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).

## <a name="azure-support-for-reverse-dns"></a>Azure ondersteuning voor omgekeerde DNS

Azure worden ondersteund in twee afzonderlijke scenario's met betrekking tot omgekeerde DNS:

1. Hosten volgens zones overeenkomt met het IP-adres tekstblok.
2. Zodat u kunt het omgekeerde DNS-record voor het IP-adres dat is toegewezen aan uw Azure-service configureren.

Ter ondersteuning van de voormalige, Azure DNS-records kan worden gebruikt voor het hosten van uw zones volgens en beheren van de PTR-records voor elk omgekeerde DNS-zoekopdracht.  Het proces van het maken van de zone volgens, de delegatie instellen en configureren van PTR-records is hetzelfde als voor de normale DNS-zones.  De enige verschillen zijn dat de overdracht moet worden ingesteld via uw Internetprovider in plaats van uw DNS-domeinregistrar en alleen de PTR-record van het type moet worden gebruikt.

Ter ondersteuning van de laatste, kunt Azure u het omgekeerde zoekactie voor het IP-adressen toegewezen aan uw service configureren.  Deze omgekeerde zoekactie is geconfigureerd door Azure als een PTR-record in de bijbehorende volgens zone.  Deze zones volgens, overeenkomt met alle de IP-bereiken gebruikt door Azure worden gehost door Microsoft. **De rest van dit artikel wordt beschreven in dit scenario uitgebreid beschreven.**
