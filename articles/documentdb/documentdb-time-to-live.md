<properties
   pageTitle="Gegevens in DocumentDB met tijd aan live verlopen | Microsoft Azure"
   description="Met TTL vindt Microsoft Azure DocumentDB u de mogelijkheid om documenten automatisch verwijderd uit het systeem na een periode."
   services="documentdb"
   documentationCenter=""
   keywords="TTL"
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/28/2016"
   ms.author="kipandya"/>

# <a name="expire-data-in-documentdb-collections-automatically-with-time-to-live"></a>Gegevens in DocumentDB verzamelingen automatisch met de tijd aan live verlopen

Toepassingen kunnen maken en opslaan van grote hoeveelheden gegevens. Sommige van deze gegevens, zoals machine gegenereerd gebeurtenis gegevens, logboeken en gebruiker sessie informatie is het alleen nuttig gedurende een beperkte periode. Zodra de gegevens wordt overtollige aan de behoeften van de toepassing deze veilig is deze gegevens wissen en de opslagbehoeften van een toepassing te verkleinen.

Met 'time to live' of TTL, vindt Microsoft Azure DocumentDB u de mogelijkheid om documenten automatisch verwijderd uit de database na een periode. De standaardtijd voor live kan worden ingesteld op het niveau van de siteverzameling en overschreven op basis van de per document. Als TTL is ingesteld, de standaardwaarde voor een siteverzameling of op het documentniveau van een, worden DocumentDB documenten die aanwezig na die periode, in seconden, aangezien ze voor het laatst zijn gewijzigd automatisch verwijderd.

TTL in DocumentDB wordt een verschoven ten opzichte van gebruikt wanneer het document het laatst is gewijzigd. Klik hiertoe site gebruikmaakt van het veld _ts dat bestaat op elk document. Het veld _ts is een unix-stijl epoche tijdstempel dat staat voor de datum en tijd. Het veld _ts wordt bijgewerkt telkens wanneer een document is gewijzigd. 

## <a name="ttl-behavior"></a>TTL gedrag

De TTL-functie wordt bepaald door de TTL-eigenschappen op twee niveaus - het niveau van de siteverzameling en documentniveau. De waarden worden ingesteld in seconden en worden behandeld als een delta uit de _ts die het document het laatst is gewijzigd op.

 1.  DefaultTTL voor de collectie
  * Als het ontbrekende (of ingesteld op null) documenten worden niet automatisch verwijderd.
  
  * Als presenteren en de waarde "-1" = oneindig-documenten niet verlopen al dan niet standaard
  
  * Als presenteren en de waarde is een nummer ("n"), maar documenten verlopen seconden "n" na de laatste wijziging

 2.  TTL voor de documenten: 
  * Eigenschap is alleen van toepassing als DefaultTTL aanwezig zijn voor de bovenliggende verzameling.
  
  * Overschrijft de DefaultTTL-waarde voor de bovenliggende verzameling.

Zodra het document is verlopen (ttl + _ts > = huidige servertijd), het document is gemarkeerd als 'verlopen'. Geen bewerking is toegestaan op deze documenten na deze tijd en de resultaten van query's uitgevoerd zij wordt uitgesloten. De documenten fysiek in het systeem zijn verwijderd en worden verwijderd in de achtergrond optioneel wordt op een later tijdstip. Dit verbruikt geen alle [Aanvragen eenheden (RUs)](documentdb-request-units.md) uit het budget van de siteverzameling.

De bovenstaande logica kan worden weergegeven in de volgende matrix:

|       | DefaultTTL instellen ontbreken/niet voor de siteverzameling | DefaultTTL = -1 in de siteverzameling | DefaultTTL = "n" in de siteverzameling|
| ------------- |:-------------|:-------------|:-------------|
| TTL ontbreekt in document| Niets negeren op documentniveau, aangezien het document en de siteverzameling hebt geen concept van TTL. | Geen documenten in deze siteverzameling verloopt. | De documenten in deze siteverzameling verloopt wanneer interval n is verstreken. |
| TTL = -1 in document | Niets negeren op het niveau van sinds de verzameling niet de DefaultTTL-eigenschap die een document kunt negeren definiëren. TTL voor een document is niet geïnterpreteerd door het systeem. | Geen documenten in deze siteverzameling verloopt. | Het document met TTL =-1 in deze verzameling verloopt nooit. Alle andere documenten verloopt na "n" interval. |
|  TTL = n in document | Niets negeren op het documentniveau van het. TTL voor een document in niet geïnterpreteerd door het systeem. | Het document met TTL = n verloopt na interval n, in seconden. Andere documenten worden overnemen interval van -1 en dat het nooit verloopt. | Het document met TTL = n verloopt na interval n, in seconden. Andere documenten overneemt van de verzameling "n" interval. |


## <a name="configuring-ttl"></a>TTL configureren

TTL is standaard uitgeschakeld al dan niet standaard in alle DocumentDB collecties en klik op alle documenten.

## <a name="enabling-ttl"></a>TTL inschakelen

Als u wilt inschakelen TTL voor een siteverzameling of de documenten in een siteverzameling, moet u de eigenschap DefaultTTL van een siteverzameling instellen op -1 of een positief getal dan nul. Als u de DefaultTTL op-1, die door de standaard die alle documenten in de verzameling eeuwen wordt live, maar de DocumentDB service moet worden gecontroleerd door deze verzameling voor documenten die deze standaard hebben overschreven.

## <a name="configuring-default-ttl-on-a-collection"></a>Standaard TTL-waarde in een siteverzameling configureren

U bent een standaardtijd voor live op het niveau van een siteverzameling configureren. 

Het instellen van de TTL-waarde voor een siteverzameling, moet u een niet-nul positief getal waarmee wordt aangegeven van de periode, in seconden, alle documenten in de verzameling na de laatste gewijzigde tijdstempel van het document (_ts) verloopt bieden.

Of u kunt de standaardkleuren instellen op -1, wat betekent dat dat alle documenten die zijn ingevoegd in aan de collectie voor onbepaalde tijd wordt live al dan niet standaard.

## <a name="setting-ttl-on-a-document"></a>TTL-instelling op een document

Naast het instellen van een standaard-TTL op een siteverzameling kunt u specifieke TTL instellen op het documentniveau van een. Hierdoor wordt overschreven door de standaardinstelling van de siteverzameling.

Als u wilt de TTL-waarde voor een document instellen, moet u een niet-nul positief getal waarmee wordt aangegeven van de periode, in seconden, verloopt het document na de laatste gewijzigde tijdstempel van het document (_ts) bieden.

Deze verschuiving verstrijken stelt het TTL-veld in te stellen voor het document.

Als een document geen TTL-veld bevat, klikt u vervolgens de standaardinstelling van de verzameling toegepast.

Als TTL is uitgeschakeld op het niveau van de siteverzameling, wordt de TTL-veld in het document genegeerd totdat TTL nogmaals op de verzameling is ingeschakeld.


## <a name="extending-ttl-on-an-existing-document"></a>TTL aan een bestaand document uitbreiden

U kunt de TTL voor een document herstellen door een bewerking schrijven op het document. Hierdoor wordt de _ts ingesteld op de huidige tijd en de aftelling tot document verloop, die wordt ingesteld door de TTL-waarde, opnieuw wordt gestart.

Als u wijzigen van de TTL-waarde van een document wilt, kunt u het veld bijwerken zoals u met een ander veld worden ingesteld doen kunt.


## <a name="removing-ttl-from-a-document"></a>TTL verwijderen uit een document

Als een TTL is ingesteld op een document en u niet langer wilt dat die document verloopt, kunt klik u ophalen van het document, verwijder het veld TTL en vervang het document op de server.

Wanneer het veld TTL is verwijderd uit het document, worden de standaardinstelling van de siteverzameling toegepast.

Als u wilt instellen dat een document verloopt en geen van de verzameling overneemt moet u de TTL-waarde instellen op -1.


## <a name="disabling-ttl"></a>TTL uitschakelen

Als u wilt TTL geheel op een verzameling uitschakelen en beëindigt u het achtergrondproces van de eigenschap DefaultTTL van de siteverzameling op zoek naar vervallen documenten moeten worden verwijderd.

Als u deze eigenschap verschilt van de eigenschap instelt op -1. Instelling van-1 betekent dat nieuwe documenten die worden toegevoegd aan de collectie eeuwen wordt live maar u kunt deze vervangen aan specifieke documenten in de siteverzameling.

Verwijderen van deze eigenschap geheel uit de verzameling betekent dat er geen documenten verlopen, zelfs als er zijn documenten die expliciet is een vorige standaard hebben overschreven.


## <a name="faq"></a>FAQ

**Wat TTL kost mij?**

Er is geen extra kosten voor het instellen van een TTL-waarde in een document.

**Hoe lang duurt het verwijderen van mijn document zodra de TTL is?**

De documenten die zijn gemarkeerd als niet beschikbaar als het document is verlopen (ttl + _ts > = huidige servertijd). Geen bewerking is toegestaan op deze documenten na deze tijd en de resultaten van query's uitgevoerd zij wordt uitgesloten. De documenten worden fysiek verwijderd door het systeem op de achtergrond. Dit wordt elke RUs uit van de verzameling budget niet gebruiken.

**Als het duurt enige tijd om de documenten te verwijderen, ze bijdragen aan mijn quotum (en de factuur) totdat ze verwijderd?**

Nee, als het document is verlopen u niet wordt gefactureerd voor de opslag van deze documenten en de grootte van de documenten worden niet meegeteld naar het opslagquotum voor een siteverzameling.

**Wordt TTL voor een document een invloed hebben op RU kosten?**

Nee, er worden geen invloed op RU kosten voor bewerkingen die zijn uitgevoerd op alle documenten binnen DocumentDB.

**Wordt het verwijderen van documenten invloed op de doorvoer die ik op mijn siteverzameling hebt ingericht?**

Nee, voldoen aan aanvragen ten opzichte van de verzameling, ontvangt prioriteit via het achtergrond gestart als u wilt verwijderen van uw documenten. TTL toevoegen aan een document, worden niet van invloed zijn op dit.

**Wanneer een document verloopt, hoe lang deze blijft in Mijn verzameling totdat deze wordt verwijderd?**

Zodra het document verloopt is deze niet meer worden gebruikt. De exacte tijd een document blijft in uw siteverzameling voordat daadwerkelijk wordt verwijderd geen is en worden gebaseerd op wanneer de achtergrond is het document verwijderen.

**Vervallen documenten verwijderd worden op alle knooppunten, of is het "uiteindelijk consistent?"**

Het document wordt niet beschikbaar op hetzelfde moment op alle knooppunten en in alle regio's.

**Is er een RU kosten voor de TTL-cmdlets voor controle achtergrondtaken?**

Nee, er is geen kosten RU hiervoor.

**Hoe vaak zijn verlopen voor TTL ingeschakeld?**

TTL verlopen voor foutcontrole optreden niet achtergrond. Bij het reageren op een verzoek om de backend-service de inline controles doet en uitsluiten van documenten die zijn verlopen. Het verwijderen van de fysieke document is het enige proces dat asynchroon op de achtergrond wordt uitgevoerd. De frequentie van dit proces wordt bepaald door de beschikbare RUs op de verzameling.

**Is de functie TTL alleen van toepassing op hele documenten of kan ik afzonderlijke document eigenschapswaarden verlopen?**

TTL is van toepassing op het hele document. Als u dat slechts een gedeelte van een document verloopt wilt, wordt klikt u vervolgens het aanbevolen dat u het gedeelte uit het hoofddocument in naar een afzonderlijk "gekoppelde" document extraheren en gebruikt u TTL aan dat uitgepakte document.

**De TTL-functie is beschikt over een specifieke indexing vereisten?**

Ja. De verzameling moet [indexeren beleid instellen](documentdb-indexing-policies.md) op Lazy of consistente. Een fout op als wilt uitschakelen op een verzameling met een al ingesteld DefaultTTL indexeren treden probeert DefaultTTL instellen op een verzameling met het indexeren is ingesteld op geen.


## <a name="next-steps"></a>Volgende stappen

Meer informatie over Azure DocumentDB, raadpleegt u de servicepagina [*documentatie*](https://azure.microsoft.com/documentation/services/documentdb/) .




