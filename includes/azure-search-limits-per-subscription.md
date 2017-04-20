U kunt meerdere services binnen een abonnement elke een deze is ingericht in een specifieke laag, alleen beperkt door het aantal services dat is toegestaan op elke laag maken. Bijvoorbeeld kon u snel aan de 12-services in de eenvoudige laag en een andere 12 services in de laag S1 binnen hetzelfde abonnement maken. Zie voor meer informatie over lagen, [Kies een SKU of niveaus voor zoekprogramma's Azure](../articles/search/search-sku-tier.md).

Beperkingen voor maximale service kunnen worden verhoogd op verzoek. Neem contact op met Azure-ondersteuning als u meer services binnen hetzelfde abonnement nodig hebt.

Resource|Vrij te geven|Eenvoudige|S1|S2|S3 |S3 HD- <sup>1</sup>
---|---|---|---|----|---|----
Maximale services |1 |12 |12  |6 |6 |6 
Maximale schaal in SU <sup>2</sup>|N/b- <sup>3</sup>|3 SU <sup>4</sup> |36 SU|36 SU|36 SU|12 SU, 3 SU <sup>5</sup>

<sup>1</sup> S3 HD biedt geen ondersteuning [indexeerfuncties](../articles/search/search-indexer-overview.md) op dit moment. 

<sup>2</sup> Zoeken-eenheden (SU) zijn factureerbare eenheden per service, als u een *replica* of een *partition*toegewezen. U nodig beide resources zijn voor de opslag, indexering en querybewerkingen. Zie voor meer informatie over de geldige combinaties die onder de beperkingen voor maximale blijven, [schaal resource niveaus voor de query en index werkbelasting](../articles/search/search-capacity-planning.md). 

<sup>3</sup> gratis is gebaseerd op gedeelde resources die worden gebruikt door meerdere abonnees. Er zijn geen speciale bronnen voor een individuele abonnee aan deze laag. Daarom is maximale schaal gemarkeerd als niet van toepassing.

<sup>4</sup> basic heeft een vaste partition. In deze laag, aanvullende SUs gebruikt voor het toewijzen van meer replica's voor betere query werkbelasting.

<sup>5</sup> S3 HD heeft een andere toewijzing-structuur met toegestane combinaties. U kunt een maximum van 12 hebben voor replica's is. Voor partities is het maximale aantal 3.




