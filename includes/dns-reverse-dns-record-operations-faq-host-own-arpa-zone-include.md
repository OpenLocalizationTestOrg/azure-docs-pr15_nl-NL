<BR> 
## <a name="faq---hosting-your-arpa-zone-in-azure-dns"></a>Veelgestelde vragen - de tijdzone volgens in Azure DNS-hostingprovider

### <a name="can-i-host-arpa-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Kan ik volgens zones voor mijn Internetprovider toegewezen IP-blokken op Azure DNS-host?
Ja. De volgens zones voor uw eigen IP-bereiken in Azure DNS-hostingprovider is volledig worden ondersteund.

Gewoon [de zone in Azure DNS maken](dns-getstarted-create-dnszone.md)en werken met uw Internetprovider aan [gemachtigde met de zone](dns-domain-delegation.md).  Vervolgens kunt u de PTR-records voor elk reverse lookup beheren op dezelfde manier als andere recordtypen.

U kunt ook [een bestaande zone voor reverse lookup met de CLI Azure importeren](dns-import-export.md).

### <a name="how-much-does-hosting-my-arpa-zone-cost"></a>Hoeveel kost die wordt hostingprovider mijn volgens zone kosten?
De zone volgens hostingprovider voor uw Internetprovider toegewezen IP-wordt blok in Azure DNS berekend [standaardtarieven Azure DNS](https://azure.microsoft.com/pricing/details/dns/).

### <a name="can-i-host-arpa-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>Kan ik volgens zones voor IPv4 en IPv6-adressen in Azure DNS-host?
Ja.