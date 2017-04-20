Een DNS-zone wordt gebruikt voor het hosten van de DNS-records voor een bepaald domein. Om te beginnen met het hosten van uw domein, moet u een DNS-zone maken. Een DNS-record hebt gemaakt voor een bepaald domein wordt deel uitmaken van een DNS-zone voor het domein. 

Het domein 'contoso.com' kan bijvoorbeeld een aantal DNS-records, zoals 'mail.contoso.com"(voor een e-mailserver) en 'www.contoso.com" (voor een website) bevatten. 


## <a name="names"></a>Informatie over DNS-zonenamen
 
- De naam van de zone moet uniek zijn binnen de resourcegroep en de zone mag niet al bestaan. Anders, mislukt de bewerking.

- Dezelfde zonenaam kan opnieuw worden gebruikt in een andere resource-groep of een ander abonnement van Azure. 

- Wanneer meerdere zones dezelfde naam delen, elk exemplaar adressen van andere naamservers worden toegewezen en slechts één exemplaar van het bovenliggende domein kan worden overgedragen. Zie [gemachtigde een domein aan Azure DNS](../articles/dns/dns-domain-delegation.md)voor meer informatie.