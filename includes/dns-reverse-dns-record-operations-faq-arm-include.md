<BR> 
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>Veelgestelde vragen - omgekeerde DNS-records voor uw Azure toegewezen IP-adres

### <a name="how-much-do-reverse-dns-records-cost"></a>Hoeveel omgekeerde DNS-records kosten?
Het fundament gratis!  Er is geen extra kosten voor omgekeerde DNS-records of query's.

### <a name="will-the-reverse-dns-records-for-my-azure-assigned-public-ip-address-resolve-from-the-internet"></a>Wordt het omgekeerde DNS-records voor mijn Azure toegewezen openbare IP-adres van internet oplossen?
Ja. Als u de omgekeerde DNS-eigenschap ingesteld voor het openbare IP-adres, beheert Azure alle DNS-delegaties en DNS-zones vereist om ervoor te zorgen dat omgekeerde DNS-record wordt herleid voor alle gebruikers van internet.

### <a name="will-a-default-reverse-dns-record-be-created-for-my-public-ip-addresses"></a>Er wordt een standaard omgekeerde DNS-record voor mijn openbare IP-adressen gemaakt?
Nee. Omgekeerde DNS is een opt-functie. Geen standaard omgekeerde DNS-records worden gemaakt als u niet wilt configureren.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Wat is de notatie voor de volledig gekwalificeerde domeinnaam (FQDN)?
FQDN's zijn opgegeven in oplopende volgorde en moeten worden afgesloten met een punt (bijvoorbeeld ' app1.contoso.com.").

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>Wat gebeurt er als de validatiecontroles voor de omgekeerde DNS-records hebt opgegeven mislukt?
Waar de validatie voor omgekeerde DNS controleert mislukt, mislukt de service management-bewerking. Corrigeer de waarde van het omgekeerde DNS-zoals vereist, en probeer het opnieuw.

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>Kan ik van mijn Website Azure omgekeerde DNS beheren?
Omgekeerde DNS wordt niet ondersteund voor Azure-Websites. Omgekeerde DNS wordt ondersteund voor Azure virtuele Machines.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-public-ip-address"></a>Kan ik meerdere omgekeerde DNS-records voor mijn openbare IP-adres configureren?
Nee. Azure ondersteunt één omgekeerde DNS-record voor elk openbare IP-adres. Elk openbare IP-adres kan wel hun eigen omgekeerde DNS-record.

### <a name="can-i-configure-reverse-dns-records-for-an-ipv6-public-ip-address"></a>Kan ik omgekeerde DNS-records configureren voor een openbare IP-adres voor IPv6?
Nee.  Op dit moment kan worden omgekeerde DNS-records ondersteund voor IPv4-adressen voor openbare IP-alleen.

### <a name="can-i-configure-a-reverse-dns-record-for-my-public-ip-address-without-having-a-domainnamelabel-specified"></a>Kan ik een omgekeerde DNS-record configureren voor mijn openbare IP-adres zonder een opgegeven DomainNameLabel?
Nee. Als u wilt gebruikmaken van omgekeerde DNS-records voor uw openbare IP-adressen, moet u de eigenschap DomainNameLabel.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Kan ik e-mailberichten naar externe domeinen van mijn services Azure berekenen verzenden?
Nee. [Azure berekenen services ondersteunen geen verzendende e-mailberichten naar externe domeinen](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).
