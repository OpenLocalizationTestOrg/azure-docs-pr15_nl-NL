Resource|Standaardbeperking|Maximumlimiet
---|---|---
Azure Media Services (AMS)-accounts in een één abonnement||25
Activa per AMS-account||1.000.000<sup>1</sup>
Gekoppelde taken per project||30
Activa per taak||50
Activa per project||100
Taken per AMS-account ||50.000<sup>2</sup>
Unieke Locator die is gekoppeld aan een actief in één keer||5<sup>4</sup>
Live kanalen per AMS-account </p></td>|5</p></td>|N/b-<sup>1</sup>
Programma's in gestopt staat per kanaal </p></td>|50</p></td>|N/b-<sup>1</sup>
Programma's in staat per kanaal uitgevoerd </p></td>|3</p></td>|3
Streaming eindpunten in staat per AMS account uitgevoerd</p></td>|2</p></td>|N/b-<sup>1</sup>
Streaming-eenheden per streaming eindpunt </p></td>|10 </p></td>|N/b-<sup>1</sup>
Tekstcodering eenheden per AMS-account </p></td>|25</p></td>|N/b-<sup>1</sup>
Opslag-accounts | |1000<sup>5</sup>
Beleidsregels || 1.000.000<sup>6</sup>

<sup>1</sup> u kunt ook de limieten voor deze quota bijwerken door het openen van een ondersteuningsticket. Maak geen meer AMS-accounts toe limieten, in plaats daarvan een ondersteuningsticket indienen.

<sup>2</sup> dit nummer bevat in de wachtrij, klaar actieve en geannuleerde taken. Het is niet inbegrepen verwijderde taken. U kunt de oude taken met **IJob.Delete** of de HTTP- **verwijderen** aanvraag verwijderen.

<sup>3</sup> wanneer u een verzoek om in de lijst taak entiteiten, een maximum van 1000 teruggeleid per aanvraag. Als u nodig hebt om alle ingediende taken bij te houden, kunt u boven/overslaan gebruiken zoals beschreven in de [OData-systeem queryopties](http://msdn.microsoft.com/library/gg309461.aspx).

<sup>4</sup> Locator zijn niet bedoeld voor het beheren van toegangsbeheer per gebruiker. Gebruiken om verschillende toegangsrechten voor individuele gebruikers, Digital Rights Management (DRM) oplossingen.

<sup>5</sup> de opslag-accounts moeten van hetzelfde Azure-abonnement.

<sup>6</sup> is er een limiet van 1.000.000 beleidsregels voor verschillende AMS beleid (bijvoorbeeld voor Locator beleid of ContentKeyAuthorizationPolicy). U moet dezelfde beleid-ID gebruiken als u altijd dezelfde dagen gebruikt / toegangsmachtigingen / enzovoort.
