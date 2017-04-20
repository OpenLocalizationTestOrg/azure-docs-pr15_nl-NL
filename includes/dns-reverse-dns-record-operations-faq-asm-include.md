<BR> 
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>Veelgestelde vragen - omgekeerde DNS-records voor uw Azure toegewezen IP-adres

### <a name="how-much-do-reverse-dns-records-cost"></a>Hoeveel omgekeerde DNS-records kosten?
Het fundament gratis!  Er is geen extra kosten voor omgekeerde DNS-records of query's.

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a>Worden mijn omgekeerde DNS-records van internet opgelost?
Ja. Als u de eigenschap van het omgekeerde DNS voor uw Cloudservice hebt ingesteld, wordt Azure beheert de DNS-delegaties en DNS-zones vereist om ervoor te zorgen dat omgekeerde DNS-record wordt herleid voor alle gebruikers van internet.

### <a name="will-a-default-reverse-dns-record-be-created-for-my-cloud-services"></a>Er wordt een standaard omgekeerde DNS-record voor mijn Cloudservices gemaakt?
Nee. Omgekeerde DNS is een opt-functie. Geen standaard omgekeerde DNS-records worden gemaakt als u niet wilt configureren.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Wat is de notatie voor de volledig gekwalificeerde domeinnaam (FQDN)?
FQDN's zijn opgegeven in oplopende volgorde en moeten worden afgesloten met een punt (bijvoorbeeld ' app1.contoso.com.").

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>Wat gebeurt er als de controles gegevensvalidatie voor de omgekeerde DNS-records hebt opgegeven mislukt?
Waar de validatie voor omgekeerde DNS controleert mislukt, mislukt de service management-bewerking. Corrigeer de waarde van het omgekeerde DNS-zoals vereist, en probeer het opnieuw.

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>Kan ik van mijn Website Azure omgekeerde DNS beheren?
Omgekeerde DNS wordt niet ondersteund voor Azure-Websites. Omgekeerde DNS wordt ondersteund voor Azure PaaS rollen en IaaS virtuele machines.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-cloud-service"></a>Kan ik meerdere omgekeerde DNS-records voor mijn Cloud-Service configureren?
Nee. Azure ondersteunt één omgekeerde DNS-record voor elke Azure-Cloudservice. Elke Cloudservice Azure kan wel hun eigen omgekeerde DNS-record.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Kan ik e-mailberichten naar externe domeinen van mijn services Azure berekenen verzenden?
Nee. [Azure berekenen services ondersteunen geen verzendende e-mailberichten naar externe domeinen](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).
