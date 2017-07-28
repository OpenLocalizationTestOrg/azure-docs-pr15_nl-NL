<properties
   pageTitle="Probeer de algemene richtlijnen | Microsoft Azure"
   description="Richtlijnen voor opnieuw voor het verwerken van de tijdelijke foutenstructuuranalyse."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/13/2016"
   ms.author="masashin"/>

# <a name="retry-general-guidance"></a>Probeer de algemene richtlijnen

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>Overzicht

Alle toepassingen die met externe services en resources communiceren moet gevoelige voor tijdelijke fouten. Dit is met name de hoofdletters/kleine letters voor toepassingen die worden uitgevoerd in de cloud, waar de aard van de omgeving en de verbinding via Internet, betekent dat dit soort fouten hebben waarschijnlijk vaker zich voordoen. Tijdelijke fouten zijn de tijdelijke verlies van netwerkconnectiviteit met onderdelen en -services, de tijdelijk niet beschikbaar van een service of de bewerkingen die zich voordoen als een service bezet is. Deze fouten zijn vaak zelf corrigeren en als de actie wordt herhaald na een vertraging geschikt is waarschijnlijk mislukt.

In dit document behandelt algemene richtlijnen voor het verwerken van de tijdelijke foutenstructuuranalyse. Zie [Azure service / regiospecifieke bericht opnieuw richtlijnen](best-practices-retry-service-specific.md)voor informatie over het afhandelen van tijdelijke fouten bij gebruik van Microsoft Azure-services.

## <a name="why-do-transient-faults-occur-in-the-cloud"></a>Waarom tijdelijke fouten die voorkomen in de cloud?

Tijdelijke fouten kunnen optreden in een omgeving, op elk platform of het besturingssysteem gebruikt en in elk type van toepassing. In oplossingen die worden uitgevoerd op lokale, infrastructuur voor on-premises implementatie, prestaties en beschikbaarheid van de toepassing en de bijbehorende onderdelen worden meestal onderhouden tot en met duur en vaak onder gebruikte hardware redundantie en onderdelen en bronnen zijn zich dicht bij elk andere. Hierdoor worden een fout minder snel, kan deze nog steeds resulteren in tijdelijke fouten- en zelfs een storing door middel van onvoorziene gebeurtenissen zoals externe voeding of netwerkproblemen of andere scenario's voor noodgevallen.

Cloud-host, waaronder persoonlijke cloud-systemen, kan een betere algehele beschikbaarheid met behulp van gedeelde resources, redundantie, automatische failover en dynamische resourcetoewijzing over een groot aantal rol berekeningscluster knooppunten bieden. De aard van deze omgevingen kan echter betekenen dat tijdelijke fouten zijn waarschijnlijk meer geneigd optreden. Er zijn verschillende redenen:

* Veel resources in een omgeving cloud worden gedeeld en toegang tot deze resources kan beperken ter bescherming van de resource. Sommige services weigert verbindingen wanneer het selectievakje laden naar een specifieke niveau groter of een tarief maximumdoorvoer is bereikt, zodat de verwerking van bestaande aanvragen en onderhouden van de service voor alle gebruikers. Beperking helpt te voor het behoud van de kwaliteit van de service voor neighbors en andere tenants met de gedeelde bron.
* Cloud omgevingen worden gemaakt met grote aantallen rol hardware-eenheden. Ze prestaties geven door het selectievakje laden dynamisch distribueren op meerdere computers eenheden en onderdelen van de infrastructuur en betrouwbaarheid geven door automatisch hergebruik of het vervangen van mislukte eenheden. Deze dynamische karakter is tijdelijke problemen en fouten tijdelijke verbinding af en toe kunnen optreden.
* Zijn er vaak meer hardwareonderdelen, waaronder netwerkinfrastructuur zoals routers en netwerktaakverdelers, tussen de toepassing en de resources en services die wordt gebruikt. Deze aanvullende infrastructuur kunt af en toe introduceren extra verbinding latentie en tijdelijke verbinding fouten.
* Netwerkcondities tussen de client en de server mogelijk variabele, met name wanneer communicatie Internet kruist. Zelfs in on-premises implementatie locaties, mogelijk zeer veel verkeer laadtijd vertraagd communicatie en niet altijd verbindingsfouten veroorzaken.

## <a name="challenges"></a>Uitdagingen
Tijdelijke fouten kunnen een grote invloed hebben op de waargenomen beschikbaarheid van een toepassing, zelfs als het goed is getest alle te verwachten omstandigheden. Om ervoor te zorgen dat cloud gehoste toepassingen betrouwbaar werken, is het moet dat ze kunnen reageren op de volgende uitdagingen:

* De toepassing moet kunnen fouten detecteren wanneer ze zich voordoen en bepalen of deze fouten zijn waarschijnlijk tijdelijke, meer langdurige of terminal mislukken. Verschillende bronnen hebben waarschijnlijk verschillende antwoorden te retourneren als een fout optreedt en deze antwoorden mogelijk ook naar gelang de context van de bewerking; bijvoorbeeld afwijken het antwoord op een fout bij het lezen van opslag van antwoord op een fout bij het schrijven naar opslag. Veel resources en services hebben goed beschreven tijdelijke storing contracten. Waar deze informatie niet beschikbaar is, is deze mogelijk wel moeilijk te vinden van de aard van de fout en of het is waarschijnlijk tijdelijke.
* De toepassing moet kunnen probeer het opnieuw als wordt bepaald dat de fout is waarschijnlijk worden tijdelijke en bijhouden hoe vaak die de bewerking opnieuw is uitgevoerd.
* De toepassing moet een passende strategie gebruiken voor de nieuwe pogingen. Deze strategie Hiermee geeft u het aantal keren deze moet proberen, de vertraging op tussen elke poging en de acties uitvoeren na een mislukte poging. Het juiste aantal pogingen en de vertraging op tussen elke een zijn vaak moeilijk te bepalen en, hangen af van het type resource, evenals de huidige besturingsomgeving voorwaarden van de resource en de toepassing zelf.

## <a name="general-guidelines"></a>Algemene richtlijnen
De volgende richtlijnen helpt u bij het ontwerpen van een geschikte tijdelijke foutenstructuuranalyse overdragen om uw toepassingen:

* **Bepalen of er een ingebouwde opnieuw om:**
  * Veel services bieden een SDK of client-bibliotheek waarin een tijdelijke foutenstructuuranalyse om voor de verwerking. Het opnieuw beleid dat wordt gebruikt, is meestal toegespitst op de aard en de vereisten van de doelservice. REST-interfaces voor services kunnen ook informatie die is nuttig zijn bij het bepalen of een nieuwe poging nodig is, en hoe lang wachten voordat u de volgende nieuwe pogingen retourneren.
  * Gebruik de ingebouwde opnieuw om waar een is beschikbaar als u hebt specifieke en bekende vereisten die leiden tot dat de werking van een andere opnieuw meer geschikt is.
* **Bepalen als de bewerking geschikt voor het opnieuw proberen is**:
  * U moet alleen bewerkingen opnieuw proberen waarin de fouten zijn tijdelijke (meestal aangegeven met de aard van de fout), en als er minimaal sommige waarschijnlijkheid dat de bewerking wordt uitgevoerd wanneer reattempted. Er is geen punt in opnieuw proberen van bewerkingen die een ongeldige bewerking zoals een database aangeven bijwerken voor een item dat niet bestaat, of die aan een service of de resource die een fatale fout heeft geleden
  * In het algemeen, moet u nieuwe pogingen alleen de volledige impact hiervan kan worden vastgesteld waarbij de voorwaarden die goed worden begrepen en kunnen worden gevalideerd implementeren. Als dat niet zo is, laat u dit op het bellen code willen pogingen om opnieuw te implementeren. Vergeet niet dat de fouten van resources en services buiten uw macht na verloop van tijd ontwikkelen mogelijk en mogelijk moet u terugkeren naar uw tijdelijke foutenstructuuranalyse detectielogica.
  * Wanneer u services of onderdelen maakt, kunt u de uitvoering van foutcodes en berichten die clients bepalen helpt of ze moeten opnieuw mislukte bewerkingen. Aangeven met name als de client moet bewerking opnieuw uitvoeren (bijvoorbeeld door een waarde **isTransient** retourneren) en een geschikte begintijd vertragen voordat het volgende nieuwe pogingen voorstellen. Als u een webservice maakt, kunt u aangepaste fouten die zijn gedefinieerd binnen uw servicecontracten worden geretourneerd. Hoewel bij een algemene clients niet mogelijk deze te lezen, worden ze handig zijn bij het maken van aangepaste-clients.
* **Een aantal juiste nieuwe pogingen en interval bepalen:**
  * Het is belangrijk om het aantal nieuwe pogingen en het interval voor het type use-case te optimaliseren. Als u een voldoende aantal keren niet proberen doen, de toepassing kan de bewerking niet voltooien en waarschijnlijk een fout. Als u opnieuw proberen te vaak of met probeert te kort een interval tussen, de toepassing kan potentieel resources zoals threads, verbindingen en geheugen bevatten voor langere, die wordt nadelig zijn voor de status van de toepassing.
  * De juiste waarden voor het tijdsinterval en het aantal nieuwe pogingen, hangt af van het type wordt geprobeerd een bewerking. Bijvoorbeeld als de bewerking deel van een gebruiker die wordt herhaald uitmaakt, het interval moet worden korte en alleen een paar pogingen probeert toe te voorkomen dat u gebruikers wachten op een reactie (die bezit open verbindingen en beschikbaarheid voor andere gebruikers kunt beperken). Als de bewerking deel van een lang actieve of kritieke-werkstroom uitmaakt, waar annuleren en opnieuw starten van het proces is duur of tijdrovende, is het geschikt zijn voor langer wachten tussen pogingen en probeer opnieuw te vaker.
  * Bepalen van de juiste intervallen tussen nieuwe pogingen, is het meest moeilijk deel van een succesvolle strategie ontwerpen. Normale strategieën gebruik de volgende soorten interval voor nieuwe pogingen:
      * **Exponentiële waarde back-uitschakelen**. De toepassing wacht even voordat de eerste opnieuw en klikt u vervolgens sterk toe met groter wordende tijden tussen elke opeenvolgende pogingen. Dit kan bijvoorbeeld probeer het opnieuw na 3 seconden, 12 seconden, 30 seconden, enzovoort.
      * **Incrementele intervallen**. De toepassing wacht even voordat de eerste opnieuw en klikt u vervolgens oplopend tijden tussen elke opeenvolgende pogingen. Dit kan bijvoorbeeld probeer het opnieuw na 3 seconden 7 seconden, 13 seconden, enzovoort.
      * **Regelmatige tussenpozen**. De toepassing wacht voor dezelfde periode van tijd tussen elke poging. Bijvoorbeeld: deze mogelijk bewerking opnieuw uitvoeren elke 3 seconden.
      * **Direct opnieuw**. Een tijdelijke fout is soms erg korte, misschien die worden veroorzaakt door een gebeurtenis, zoals een conflict netwerk-pakket of een Prikker in een hardware-onderdeel. In dit geval is het opnieuw direct nodig omdat deze van slagen kans als de fout in de tijd die het duurt de toepassing samenstellen en verzend het verzoek van de volgende heeft uitgeschakeld. Echter moet nooit er meer dan één direct nieuwe pogingen, en moet u overschakelen naar alternatieve strategieën, zoals zoals exponentiële back- of uitschakelen fallback acties, als de onmiddellijke poging mislukt.
      * **Aselecte indeling**. Een van de nieuwe poging strategieën hierboven wordt mogelijk een aselecte indeling om te voorkomen dat meerdere exemplaren van de client verzenden van de volgende nieuwe pogingen op hetzelfde moment opnemen. Bijvoorbeeld één exemplaar mogelijk de bewerking opnieuw proberen na 3 seconden, 11 seconden 28 seconden en enzovoort terwijl een ander exemplaar mogelijk probeer het opnieuw na 4 seconden 12 seconden, 26 seconden, enzovoort. Aselecte indeling is een handige techniek die mogelijk worden gecombineerd met andere strategieën.  
  * Normaal gesproken, gebruikt u een exponentiële strategie back-uitschakelen voor bewerkingen op de achtergrond en onmiddellijk of gewone interval opnieuw strategieën voor interactieve bewerkingen. In beide gevallen moet u de vertraging en het aantal nieuwe pogingen zodat de maximale duur geldt voor alle herhalingspogingen is binnen de vereiste vereist end-to-end latentie.
  * Rekening houden met de combinatie van alle factoren die aan de totale maximale time-out voor een opnieuw geprobeerde bewerking bijdragen. Deze factoren zijn de duur van een kan geen verbinding met een reactie (meestal ingesteld door een time-outwaarde in de client) produceren evenals de vertraging op tussen nieuwe pogingen en het maximum aantal pogingen. Het totaal van alle deze tijden kan resulteren in zeer grote algehele bewerking tijden, met name wanneer een exponentiële vertraging strategie gebruiken waar het interval tussen pogingen in omvang snel na elke is mislukt groeit. Als een proces moet overeenkomen met een specifieke SLA service level agreement (), moet de totale bewerkingstijd, inclusief alle-outs en vertragingen, worden binnen die gedefinieerd in de SERVICEOVEREENKOMST
  * Over-Aggressive opnieuw strategieën, die te kort intervallen of te kunnen pogingen, kunnen een nadelige gevolgen hebben voor de doeltoepassing bron of service. Hiermee kan verhinderen dat de bron of service herstellen uit de overbelasting staat en wordt gewoon blokkeren of weigeren van aanvragen. Deze resultaten in een vicious cirkel waar meer aanvragen worden verzonden naar de resource of service en daardoor de mogelijkheid om te herstellen nog verder beperkt.
  * Rekening de time-out van de bewerkingen bij het kiezen van de intervallen voor nieuwe pogingen om te voorkomen met het starten van een volgende poging direct (bijvoorbeeld als de time-out is vergelijkbaar met het interval voor nieuwe pogingen). Ook rekening houden als u wilt bewaren van de totale mogelijke periode (de time-out plus de intervallen voor nieuwe pogingen) tot onder een specifieke totale tijd. Bewerkingen die u ongebruikelijk korte of lange time-out hebt invloed kunnen zijn op hoe lang om te wachten en hoe vaak u probeer het opnieuw.
  * Gebruik van het type de uitzondering en alle gegevens in de cel of de foutcodes en berichten als resultaat gegeven van de service, om het interval en het aantal pogingen te optimaliseren. Voor bijvoorbeeld sommige uitzonderingen of fout codes (zoals de HTTP code 503 Service niet beschikbaar met een koptekst opnieuw na in het antwoord) aangeven mogelijk hoe lang de fout kan duren, of die wordt de service is mislukt en wordt niet reageert op de volgende pogingen.
* **Vermijd tegen patronen**:
  * In de meeste gevallen Vermijd implementaties die gedupliceerde lagen van opnieuw code bevatten. Ontwerpen die trapsgewijze opnieuw regelingen bevatten of die implementeren opnieuw in elke fase van een bewerking die betrekking heeft op een hiërarchie van aanvragen, tenzij u over de vereisten die aanvraag hebt kunt dit voorkomen. In deze uitzonderlijke omstandigheden beleid die voorkomen dat overtollige aantal pogingen en vertraging perioden en controleer of dat u de gevolgen te gebruiken. Als één onderdeel een nieuw vergaderverzoek naar een andere maakt, probeert welke vervolgens toegang tot de doelservice en u opnieuw met een telling van drie beide oproepen er implementeren negen opnieuw bijvoorbeeld in totaal ten opzichte van de service. Veel services en resources implementeren een om ingebouwde opnieuw en moet u hoe u kunt uitschakelen of wijzigen als u wilt implementeren pogingen op een hoger niveau onderzoeken.
  *  Nooit een om eindeloos opnieuw implementeren. Dit is waarschijnlijk om te voorkomen dat de bron of service herstellen overbelasting situaties en ertoe leiden dat beperken en verbindingen om door te gaan voor een langere periode geweigerd. Gebruik een eindige nummer of nieuwe pogingen of implementeren van een patroon zoals [stroomonderbreker](http://msdn.microsoft.com/library/dn589784.aspx) toe te staan dat de service te herstellen.
  * Nooit meer dan één keer een direct opnieuw uitvoeren.
  * Vermijd het gebruik van een gewone opnieuw interval, met name als er een groot aantal nieuwe pogingen, bij het openen van de services en bronnen in Azure wordt aangegeven. De optimale benadering is dat dit scenario is een exponentiële back-uitschakelen strategie met een mogelijkheid circuitlijnen recente.
  * Voorkomen dat meerdere exemplaren van dezelfde client of meerdere exemplaren van verschillende clients van pogingen om opnieuw te verzenden op hetzelfde moment. Als dit kunnen optreden, introduceren aselecte indeling in de intervallen opnieuw.
* **Test uw strategie opnieuw en -implementatie:**
  * Controleer of dat u de implementatie van de strategie opnieuw onder als breed een set omstandigheden mogelijk volledig test met name wanneer zowel de toepassing en het doel resources of services die wordt gebruikt onder extreme laden zijn. Als u wilt controleren gedrag tijdens testen, kunt u het volgende doen:
      * Tijdelijke en tijdelijke fouten invoeren in de service. Bijvoorbeeld verzenden ongeldige aanvragen of code die wordt gedetecteerd test aanvraagt en reageert met verschillende soorten fouten toe te voegen. Een voorbeeld via TestApi, leest u [Foutenstructuuranalyse webweergave testen met TestApi](http://msdn.microsoft.com/magazine/ff898404.aspx) en [Inleiding tot TestApi – deel 5: beheerde Code foutenstructuuranalyse webweergave API's](http://blogs.msdn.com/b/ivo_manolov/archive/2009/11/25/9928447.aspx).
      * Maak een nagebootste van de resource of de service die resulteert in een bereik van fouten die de reële-service kan retourneren. Controleer of u alle typen fout voorblad dat uw strategie opnieuw is ontworpen om te bepalen.
      * Tijdelijke fouten te voorkomen door tijdelijk uitschakelen of de service overbelasting als dit is een aangepaste service die u hebt gemaakt en geïmplementeerd afdwingen (u moet, natuurlijk niet overbelastingen gedeelde bronnen of gedeelde services binnen Azure).
      * Voor HTTP gebaseerde API's, kunt u overwegen de FiddlerCore-bibliotheek in uw automatische tests te wijzigen van het resultaat van HTTP-aanvragen, door het toevoegen van extra retour tijden of door het wijzigen van het antwoord (zoals de HTTP-statuscode, kopteksten, hoofdtekst of andere factoren). Hierdoor kunnen deterministic testen van een subset van de voorwaarden mislukt of tijdelijke fouten of andere soorten is mislukt. Zie [FiddlerCore](http://www.telerik.com/fiddler/fiddlercore)voor meer informatie. Voor voorbeelden over het gebruik van de bibliotheek, met name de **HttpMangler** -klasse, controleert u de [broncode voor de SDK van Azure opslag](https://github.com/Azure/azure-storage-net/tree/master/Test).
      * Hoge belasting factor en gelijktijdige tests om ervoor te zorgen dat de opnieuw om en strategie werkt correct onder deze voorwaarden voldoet: en niet heeft een nadelige gevolgen voor de werking van de client of ertoe leiden dat kruisbesmetting tussen aanvragen uitvoeren.
* **Opnieuw beleidsconfiguraties beheren:**
  * Een _beleid opnieuw_ is een combinatie van alle elementen van uw strategie opnieuw. Hiermee definieert u de detectie om die bepaalt of een fout is waarschijnlijk tijdelijke, het type interval om te gebruiken (zoals normale, exponentiële back-uitschakelen en aselecte indeling), de werkelijke interval waarde(n) en het aantal keren opnieuw uit te voeren.
  * Nieuwe pogingen moeten worden geïmplementeerd op vele plaatsen op zelfs de eenvoudigste toepassing en in elke laag van complexere toepassingen. In plaats van harde codering de elementen van elk beleid op meerdere locaties, kunt u overwegen een middenpunt voor het opslaan van alle beleidsregels. Bijvoorbeeld opslaan van de waarden zoals het interval tellen in configuratie toepassingsbestanden opnieuw, ze gedurende runtime lezen en het beleid opnieuw via een programma te maken. Dit eenvoudiger om de instellingen, te beheren en de waarden deze af te stellen om te reageren op veranderende vereisten en scenario's en wijzigen. Echter ontwerp het systeem naar de waarden kunt opslaan in plaats van opnieuw gelezen een configuratie telkens bij het bestand en zorg ervoor dat geschikt standaardwaarden worden gebruikt als de waarden niet uit de configuratie ophalen.
  * In een Azure Cloud Services-toepassing, kunt u het opslaan van de waarden die worden gebruikt voor het maken van het beleid opnieuw gedurende runtime in het configuratiebestand service, zodat ze kunnen worden gewijzigd zonder dat u nodig hebt om de toepassing opnieuw te starten.
  * Maak gebruik van de ingebouwde of standaardinhoudstype opnieuw strategieën beschikbaar in de client-API's die u gebruikt, maar alleen wanneer ze geschikt voor uw scenario zijn. Deze strategieën zijn meestal algemene. In sommige gevallen ze mogelijk alle die is vereist, maar in andere scenario's niet bieden de volledige reeks opties aanpassen aan uw specifieke vereisten. Hoe de instellingen voor uw toepassing door middel van testen om te bepalen de meest geschikte waarden worden weergegeven op, moet u begrijpen.
* **Meld u aan en bijhouden van tijdelijke en tijdelijke fouten:**
  * Als onderdeel van uw strategie opnieuw zijn afhandeling van uitzonderingen en andere instrumentatie die Logboeken wanneer nieuwe pogingen worden aangebracht. Terwijl een incidentele tijdelijke storing en probeer het opnieuw te verwachten, en wijzen niet op een probleem, regelmatige en toeneemt aantal pogingen om opnieuw te vaak een aanduiding van een probleem met een fout kan veroorzaken of is momenteel die invloed hebben op prestaties en beschikbaarheid zijn.
  * Meld u tijdelijke fouten als waarschuwing gegevens in plaats van foutieve posten zodat systemen voor toezicht niet door deze als toepassingsfouten die mogelijk ONWAAR waarschuwingen wordt geactiveerd.
  * Houd rekening met een waarde in uw logboekvermeldingen waarmee wordt aangegeven als de nieuwe pogingen zijn die worden veroorzaakt door beperken in de service of andere soorten fouten zoals verbindingsfouten, zodat u ze kunt onderscheiden tijdens de analyse van de gegevens opslaan. Een stijging van het aantal bandbreedteregeling fouten wordt vaak een aanduiding van een fout ontwerpen in de toepassing of dat u wilt overschakelen naar een premium-service met speciale hardware.  
  * Overweeg om te meten en vastleggen van de totale tijd voor bewerkingen die functie opnieuw. Dit is een goede indicator van de algehele effect van tijdelijke fouten op gebruiker antwoord tijden, proces latentie en de efficiëntie van de toepassing gebruik gevallen. Ook opgetreden log het aantal pogingen om te begrijpen de factoren die van aan de tijd antwoord bijgedragen.
  * Maak eventueel een telemetrielogboek implementeren en monitoring systeem dat berichten wanneer het nummer kunt verhogen en percentage mislukte, het gemiddelde aantal pogingen is of de algehele tijden die u hebt gemaakt voor bewerkingen alleen mogelijk als toeneemt.
* **Bewerkingen die continu mislukt beheren:**
  * Er zijn omstandigheden waarin de bewerking blijft mislukken bij elke poging en houd rekening met hoe u deze situatie omgaan noodzakelijk is:
      * Hoewel een strategie opnieuw het maximum aantal keren dat een bewerking opnieuw moet worden uitgevoerd definiëren wordt, voorkomt deze niet de toepassing bewerking Klik nogmaals met hetzelfde aantal pogingen om opnieuw te herhalen. Bijvoorbeeld als een order processing service met een fatale fout permanent buiten werking plaatst mislukt, de strategie opnieuw mogelijk een time-out van de verbinding wordt gedetecteerd en beschouwen een tijdelijke veroorzaakt. De code wordt de bewerking een opgegeven aantal keren opnieuw proberen en geef omhoog. Echter wanneer een andere klant een order plaatst, wordt de bewerking geprobeerd opnieuw - ook al is dit ervoor dat u elke keer mislukt.
      * Om te voorkomen dat continu pogingen voor bewerkingen die continu mislukt, kunt u het [patroon stroomonderbreker](http://msdn.microsoft.com/library/dn589784.aspx)implementeren. In dit patroon als het aantal mislukte binnen een opgegeven tijdvenster valt groter is dan de drempelwaarde voor, worden aanvragen geretourneerd naar de beller onmiddellijk als fouten zonder poging tot toegang tot de mislukte bron of service.
      * De toepassing kunt regelmatig de service, op basis van periodieke en met erg lang intervallen tussen aanvragen, om te bepalen wanneer deze beschikbaar testen. Een juiste interval, is afhankelijk van het scenario, zoals de ernst van de bewerking en de aard van de service, en mogelijk iets een paar minuten tot enkele uren. Op het punt waar de test is geslaagd, kan de toepassing hervatten normale bewerkingen en aanvragen doorgeven aan de zojuist herstelde service.
      * Ondertussen is het mogelijk terugschakelen naar een ander exemplaar van de service (bijvoorbeeld in een ander datacenter of toepassing), gebruikt u een soortgelijke service die compatibel (misschien eenvoudiger) functionaliteit biedt of bepaalde alternatieve bewerkingen uitvoeren in de verwachting dat de service beschikbaar binnenkort komen. Bijvoorbeeld mogelijk geschikt zijn voor het opslaan van aanvragen voor de service in een wachtrij of gegevens opslaan en deze later te herhalen. Anders u mogelijk de gebruiker omleiden naar een alternatief exemplaar van de toepassing kan de prestaties van de toepassing afnemen maar nog steeds aanvaardbaar functionaliteit bieden of alleen afzender een bericht naar de gebruiker die aangeeft dat de toepassing niet beschikbaar is binnen presenteren.

* **Andere relevante informatie**
  * Bij het kiezen van de waarden voor het aantal pogingen en de intervallen voor nieuwe pogingen voor een beleid, kunt u als de bewerking op de service of de resource deel van een bewerking langdurige of meerdere stappen uitmaakt. Het kan lastig of dure ter alle andere operationele stappen die u hebt al is geslaagd wanneer een mislukt. In dit geval een erg lang tijdsinterval en een groot aantal pogingen mogelijk aanvaardbaar zo lang maken als andere bewerkingen niet worden geblokkeerd door vasthouden of schaarse vergrendelen.
  * Houd rekening met als het dezelfde opnieuw gegevens inconsistent mogelijk. Als sommige onderdelen van een meerdere stappen worden herhaald en de bewerkingen niet idempotency is ingeschakeld zijn, kan dit resulteren in een inconsistenties. Bijvoorbeeld zullen een bewerking die wordt een waarde, verhoogd als herhaald, produceren een ongeldig resultaat. Een bewerking die een bericht naar een wachtrij stuurt herhalende mogelijk veroorzaakt een inconsistenties in de bericht-gebruiker als deze kan geen dubbele berichten vinden. U kunt dit voorkomen, zorg ervoor dat u elke stap als een bewerking idempotency is ingeschakeld ontwerpt. Zie voor meer informatie over idempotency, [Idempotency patronen](http://blog.jonathanoliver.com/2010/04/idempotency-patterns/).
  * Houd rekening met het bereik van de bewerkingen die wordt opnieuw geprobeerd. Bijvoorbeeld: deze mogelijk beter te implementeren opnieuw code op een niveau die meerdere bewerkingen omvat en probeer opnieuw of ze allemaal als een mislukt. Echter Hierdoor kan dit leiden tot idempotency problemen of onnodige terugdraaien bewerkingen.
  * Als u een opnieuw-instellen dat een combinatie van verschillende bewerkingen kiest, rekening houden met het totale latentie van alle tabelstijlen bij het bepalen van de intervallen voor nieuwe pogingen, bij het controleren van de tijd die u hebt gemaakt en voordat u meldingen voor niet-werkende.
  * Houd rekening met uw strategie opnieuw invloed neighbors en andere tenants in een gedeelde toepassing of wanneer u een gedeelde resources en -services gebruikt. Agressieve opnieuw beleidsregels kunnen leiden tot steeds meer tijdelijke problemen voordoen bij deze andere gebruikers en voor toepassingen die delen van de resources en services. Uw toepassing kan ook worden beïnvloed door het beleid opnieuw is geïmplementeerd door andere gebruikers van de resources en -services. Voor belangrijke toepassingen, kunt u besluiten gebruik Premieservices die niet worden gedeeld. Dit biedt veel meer controle over de laden en na een beperking van deze resources en services die kunnen u de extra kosten uitvullen.

## <a name="more-information"></a>Meer informatie

* [Azure-service / regiospecifieke opnieuw richtlijnen](best-practices-retry-service-specific.md)
* [De blokkering van de toepassing tijdelijke foutenstructuuranalyse afhandeling](http://msdn.microsoft.com/library/hh680934.aspx)
* [Stroomonderbreker patroon](http://msdn.microsoft.com/library/dn589784.aspx)
* [Transactie patroon verwacht](http://msdn.microsoft.com/library/dn589804.aspx)
* [Idempotency patronen](http://blog.jonathanoliver.com/2010/04/idempotency-patterns/)