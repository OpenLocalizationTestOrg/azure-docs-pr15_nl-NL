<properties
   pageTitle="Synchronisatie van Azure AD Connect: inzicht in de architectuur | Microsoft Azure"
   description="In dit onderwerp worden de architectuur van Azure AD Connect-synchronisatie en wordt uitgelegd de voorwaarden die worden gebruikt."
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/31/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-understanding-the-architecture"></a>Synchronisatie van Azure AD Connect: inzicht in de architectuur
Dit onderwerp behandelt de eenvoudige architectuur voor Azure AD Connect synchroniseren. In veel aspecten is vergelijkbaar met de voorafgaande taken MIIS 2003, 2007 ILM en FIM 2010. Azure AD Connect-synchronisatie is de ontwikkeling van deze technologieën. Als u bekend met een van deze eerdere technologieën bent, wordt de inhoud van dit onderwerp als u ook bij u bekend zijn. Als u eerder met synchronisatie, klikt u vervolgens in dit onderwerp is bedoeld voor u. Het is echter niet een vereiste informatie van de details van dit onderwerp lukt bij het maken van aanpassingen vereist voor synchronisatie van Azure AD Connect (synchronisatie-engine in dit onderwerp genoemd).

## <a name="architecture"></a>Architectuur
De synchronisatie-engine een geïntegreerde weergave van objecten die zijn opgeslagen uit meerdere gegevensbronnen, verbonden maakt en beheert identiteit informatie in die gegevensbronnen. Deze geïntegreerde weergave wordt bepaald door de identiteit informatie opgehaald uit verbonden gegevensbronnen en een set regels die bepalen hoe u deze informatie verwerken.

### <a name="connected-data-sources-and-connectors"></a>Verbonden gegevensbronnen en verbindingslijnen
De synchronisatie-engine verwerkt identiteitsgegevens uit verschillende gegevens opslagplaatsen, zoals Active Directory of een SQL Server-database. Elke gegevensopslagplaats die de gegevens in een indeling van de database-achtige organiseert en die standaard gegevenstoegang manieren is een mogelijke gegevensbron kandidaten voor de synchronisatie-engine. De gegevens opslagplaatsen die zijn gesynchroniseerd met de synchronisatie-engine worden **verbonden gegevensbronnen** of **mappen verbonden** (CD) genoemd.

De synchronisatie-engine omvat interactie met een verbonden gegevensbron binnen een module met de naam van een **verbindingslijn**. Elk type verbonden gegevensbron heeft een bepaalde verbindingslijn. De verbindingslijn equivalent een vereiste bewerking in de indeling die de gekoppelde gegevensbron begrijpt.

Verbindingslijnen bellen API identiteit informatie (zowel lezen en schrijven) van exchange met een verbonden gegevensbron. Het is ook mogelijk om toe te voegen een aangepaste verbindingslijn met het framework extensible connectivity. De volgende afbeelding ziet hoe de bron van een gekoppelde gegevens in een verbindingslijn maakt verbinding met de synchronisatie-engine.

![Arch1](./media/active-directory-aadconnectsync-understanding-architecture/arch1.png)

Gegevens in een willekeurige richting kunnen flow, maar deze niet kunt flow in beide richtingen tegelijk. Met andere woorden, een verbindingslijn kan worden geconfigureerd om te kunnen gegevens van de verbonden gegevensbron synchronisatie-engine of van de synchronisatie-engine met de verbonden gegevensbron, maar alleen een van deze bewerkingen kan voorkomen op elk gewenst moment één voor één object en kenmerk. De richting kan afwijken van verschillende objecten en andere kenmerken.

Als u wilt een verbindingslijn configureren, moet u de objecttypen die u wilt synchroniseren opgeven. Hiermee definieert u de objecttypen geven het bereik van objecten die van het synchronisatieproces uitmaken deel. De volgende stap is om te selecteren van de kenmerken worden gesynchroniseerd, die is bekend als een lijst met kenmerken deze op te nemen. Deze instellingen kunnen elk gewenst moment in antwoord op wijzigingen aan uw bedrijfsregels worden gewijzigd. Wanneer u de installatiewizard Azure AD Connect gebruikt, worden deze instellingen zijn geconfigureerd voor u.

Als u wilt exporteren objecten met een verbonden gegevensbron, moet het kenmerklijst ten minste de kenmerken die is vereist voor het maken van een specifiek type in een verbonden gegevensbron. De **sAMAccountName** -kenmerk moet bijvoorbeeld worden opgenomen in de kenmerklijst deze op te nemen aan een gebruikersobject exporteren naar Active Directory, omdat alle gebruikersobjecten in Active Directory een gedefinieerd **sAMAccountName** -kenmerk moeten hebben. Klik nogmaals biedt de installatiewizard deze configuratie voor u.

Als de verbonden gegevensbron wordt gebruikt voor structurele onderdelen, zoals partities of containers ordenen van objecten, kunt u de gebieden in de verbonden gegevens die worden gebruikt voor een bepaald oplossing beperken.

### <a name="internal-structure-of-the-sync-engine-namespace"></a>Interne structuur van de synchronisatie-engine naamruimte
De volledige synchronisatie-engine naamruimte bestaat uit twee naamruimten die de informatie over identiteit opslaan. De twee naamruimten zijn:

- De verbindingslijn ruimte (CS)
- De metaverse (MV)

De **verbindingslijn ruimte** is tijdelijke opslag met weergaven van de aangewezen objecten uit een bron gekoppelde gegevens en de kenmerken die zijn opgegeven in de lijst met kenmerken deze op te nemen. De synchronisatie-engine gebruikt de ruimte verbindingslijn om te bepalen wat heeft veranderd in de verbonden gegevensbron en binnenkomende wijzigingen van fase. De synchronisatie-engine ook wordt de ruimte verbindingslijn fase uitgaande wijzigingen voor exporteren naar de verbonden gegevensbron. De synchronisatie-engine onderhoudt een spatie distinct connector als tijdelijke opslag voor elke verbindingslijn.

Met behulp van tijdelijke opslag, is de synchronisatie-engine blijft onafhankelijk van de verbonden gegevensbronnen, en wordt niet beïnvloed door hun beschikbaarheid en de toegankelijkheid. U kunt daardoor identiteit informatie op elk gewenst moment verwerken met behulp van de gegevens in het opslaggebied. De synchronisatie-engine kunt alleen de wijzigingen in de verbonden gegevensbron sinds de laatste communicatie-sessie beëindigd of push om alleen de wijzigingen in identiteit informatie die de verbonden gegevensbron nog niet ontvangen, waardoor het netwerkverkeer tussen de synchronisatie-engine en de verbonden gegevensbron aanvragen.

Synchronisatie-engine opgeslagen bovendien statusinformatie over alle objecten dat deze in de ruimte verbindingslijn fasen. Wanneer u nieuwe gegevens wordt ontvangen, evalueert synchronisatie-engine altijd of de gegevens al is gesynchroniseerd.

De **metaverse** is een opslagruimte met de samengevoegde identiteitsgegevens uit meerdere verbonden gegevensbronnen, één globale, geïntegreerde weergave van alle gecombineerde objecten leveren. Metaverse objecten zijn gemaakt op basis van de identiteitsgegevens die zijn opgehaald uit de verbonden gegevensbronnen en een set regels waarmee u kunt het aanpassen van het synchronisatieproces.

De volgende afbeelding ziet u de verbindingslijn ruimte naamruimte en de naamruimte metaverse binnen de synchronisatie-engine.

![Arch2](./media/active-directory-aadconnectsync-understanding-architecture/arch2.png)

## <a name="sync-engine-identity-objects"></a>Synchronisatie-engine identiteit objecten
De objecten in de synchronisatie-engine zijn in de vorm van een van beide objecten in de verbonden gegevensbron of de geïntegreerde weergave die synchroniseren engine heeft deze objecten. Elke sync engine-object moet een globaal unieke id (GUID). GUID's bieden gegevensintegriteit en express relaties tussen objecten.

### <a name="connector-space-objects"></a>Connector ruimteobjecten
Wanneer de synchronisatie-engine communiceert met een verbonden gegevensbron, leest de identiteit informatie in de verbonden gegevensbron en gegevens gebruikt om een weergave van het object identiteit maken in de ruimte verbindingslijn. U kunt maken of deze objecten afzonderlijk verwijderen. U kunt alle objecten in een spatie verbindingslijn echter handmatig verwijderen.

Alle objecten in de ruimte verbindingslijn hebben twee kenmerken:

- Een globaal unieke id (GUID)
- Een DN (ook wel bekend als Distinguished name)

Als de verbonden gegevensbron wordt een uniek kenmerk aan het object toegewezen, kunnen klikt u vervolgens objecten in de ruimte verbindingslijn ook bevatten een ankerkenmerk. Het ankerkenmerk is een unieke aanduiding van een object in de verbonden gegevensbron. De synchronisatie-engine gebruikt het anker om te zoeken van de bijbehorende weergave van dit object in de verbonden gegevensbron. Synchronisatie-engine wordt ervan uitgegaan dat het anker van een object nooit gewijzigd in de loop van het object.

Veel van de verbindingslijnen een bekende unieke id gebruikt om een anker automatisch voor elk object genereren wanneer deze wordt geïmporteerd. Het Active Directory-Connector worden bijvoorbeeld het kenmerk **objectGUID** voor een anker gebruikt. Voor verbonden gegevensbronnen die niet in een duidelijk omschreven unieke id voorzien, kunt u anker generatie opgeven als onderdeel van de configuratie van de Connector.

In dat geval het anker wordt opgebouwd uit een of meer unieke kenmerken van een object, noch van welke wijzigingen en unieke typt geeft aan wat het object in de ruimte verbindingslijn (bijvoorbeeld een nummer of een gebruikers-ID).

Een verbindingslijn ruimte object zijn van de volgende opties:

- Een object tijdelijk opslaan
- Een tijdelijke aanduiding

### <a name="staging-objects"></a>Tijdelijke objecten
Een tijdelijk opslaan object vertegenwoordigt een exemplaar van de aangewezen objecttypen uit de bron gekoppelde gegevens. Naast de GUID en de DN-naam heeft een tijdelijk opslaan object altijd een waarde die aangeeft van het objecttype.

Tijdelijke objecten die zijn geïmporteerd altijd bevatten een waarde voor het ankerkenmerk. Tijdelijke objecten die nieuw zijn voorzien door de synchronisatie-engine en zijn nog moet worden gemaakt in de verbonden gegevensbron hebben geen een waarde voor het ankerkenmerk.

Tijdelijke objecten ook uitvoeren huidige waarden van bedrijven kenmerken en meer informatie nodig zijn voor de synchronisatie-engine om uit te voeren van het synchronisatieproces bewerkingen. Operationele informatie bevat vlaggen die aangeven welke typen updates die zijn gefaseerd op het tijdelijk opslaan object. Als een tijdelijk opslaan object heeft ontvangen van nieuwe identiteit informatie uit de verbonden gegevensbron die nog niet is verwerkt, wordt het object is gemarkeerd als **in behandeling importeren**. Als een tijdelijk opslaan object nieuwe identiteit informatie die nog niet is geëxporteerd naar de verbonden gegevensbron heeft, wordt het gemarkeerd als **in behandeling exporteren**.

Een tijdelijk opslaan object kan ook een object importeren of een object exporteren. De synchronisatie-engine Hiermee maakt u een object importeren met behulp van Objectinformatie hebt ontvangen van de verbonden gegevensbron. Wanneer synchronisatie-engine informatie over het bestaan van een nieuw object die overeenkomt met een van de objecttypen in de verbindingslijn is geselecteerd ontvangt, wordt een object importeren in de ruimte connector als een weergave van het object in de verbonden gegevensbron.

De volgende afbeelding ziet u een object importeren die een object in de verbonden gegevensbron vertegenwoordigt.

![Arch3](./media/active-directory-aadconnectsync-understanding-architecture/arch3.png)

De synchronisatie-engine Hiermee maakt u een object exporteren met behulp van objectinformatie in de metaverse. Exporteren objecten worden geëxporteerd naar de verbonden gegevensbron tijdens de volgende communicatie. Vanuit het perspectief van de synchronisatie-engine bestaan exporteren objecten niet in de verbonden gegevensbron nog. Het ankerkenmerk voor een object exporteren dus niet beschikbaar. Nadat u deze het object van synchronisatie-engine ontvangt, wordt een unieke waarde voor het ankerkenmerk van het object in de verbonden gegevensbron gemaakt.

De volgende afbeelding ziet hoe een object exporteren wordt gemaakt met behulp van identiteit informatie in de metaverse.

![Arch4](./media/active-directory-aadconnectsync-understanding-architecture/arch4.png)

De synchronisatie-engine bevestigt de export van het object door het object uit de bron gekoppelde gegevens opnieuw te importeren. Exporteren objecten worden objecten importeren terwijl synchronisatie-engine deze tijdens het volgende importeren uit die verbonden gegevensbron ontvangt.

### <a name="placeholders"></a>Tijdelijke aanduidingen
De synchronisatie-engine gebruikt een platte naamruimte voor de opslag van objecten. Echter enkele verbonden gegevensbronnen, zoals Active Directory een hiërarchische naamruimte gebruiken. Synchronisatie-engine om gegevens uit een hiërarchische naamruimte omzetten in een platte naamruimte, worden tijdelijke aanduidingen voor het behoud van de hiërarchie gebruikt.

Elke tijdelijke aanduiding een onderdeel (bijvoorbeeld een organisatie-eenheid) van hiërarchische naam van een object dat niet in de synchronisatie-engine is ingevoerd, maar is vereist om de naam van de hiërarchische te maken. Gemaakt door verwijzingen in de verbonden gegevensbron naar objecten die zijn geen objecten in de ruimte verbindingslijn tijdelijke voor hiaten in te vullen.

De synchronisatie-engine wordt ook gebruikt voor tijdelijke aanduidingen voor de opslag van objecten waarnaar wordt verwezen die nog niet zijn geïmporteerd. Als synchronisatie is geconfigureerd voor het opnemen van de manager-kenmerk voor het object *Abbie Spencer* en de ontvangen waarde is bijvoorbeeld een object dat niet geïmporteerd nog, zoals is *CN = Lee Sperry, CN = gebruikers, domeincontroller = fabrikam, domeincontroller = com*, de manager-gegevens worden opgeslagen als tijdelijke aanduidingen in de ruimte verbindingslijn. Als het managerobject later wordt geïmporteerd, wordt de tijdelijke aanduiding voor object wordt overschreven door het tijdelijk opslaan object dat de manager vertegenwoordigt.

### <a name="metaverse-objects"></a>Metaverse objecten
Een object metaverse de geaggregeerde weergave bevat die synchronisatie-engine heeft het tijdelijk opslaan objecten in de ruimte verbindingslijn. Synchronisatie-engine wordt metaverse objecten gemaakt met behulp van de informatie in objecten importeren. Verschillende verbindingslijn ruimteobjecten kunnen worden gekoppeld aan een object één metaverse, maar een verbindingslijn ruimte object kan niet worden gekoppeld aan meer dan één metaverse object.

Metaverse objecten kunnen niet handmatig worden gemaakt of verwijderd. De synchronisatie-engine worden automatisch verwijderd metaverse objecten waarvoor geen een koppeling naar een verbindingslijn ruimte-object in de ruimte verbindingslijn.

Objecten in een verbonden gegevensbron om aan te wijzen een bijbehorende objecttype binnen de metaverse, biedt de synchronisatie-engine een uitgebreid schema met een vooraf gedefinieerde set objecttypen en de bijbehorende kenmerken. U kunt nieuwe objecttypen en kenmerken voor metaverse objecten maken. Kenmerken kunnen werken met één waarde of met meerdere waarden en de kenmerktypen kunnen zijn tekenreeksen, verwijzingen, getallen en Booleaanse waarden.

### <a name="relationships-between-staging-objects-and-metaverse-objects"></a>Relaties tussen tijdelijk opslaan en metaverse objecten
Binnen de synchronisatie-engine naamruimte, is de gegevensstroom ingeschakeld door de koppeling relatie tussen tijdelijk opslaan en metaverse objecten. Een tijdelijk opslaan object dat is gekoppeld aan een object metaverse heet een **die zijn gekoppeld object** (of **verbindingslijn object**). Een tijdelijk opslaan object dat niet is gekoppeld aan een object metaverse heet een **niet langer lid object** (of **disconnector object**). De voorwaarden die zijn gekoppeld en niet langer lid voorkeur aan niet verwarren met de verbindingslijnen die verantwoordelijk is voor het importeren en exporteren van gegevens vanuit een verbonden adreslijst.

Tijdelijke aanduidingen zijn nooit gekoppeld aan een object metaverse

Een gekoppelde object bestaat uit een tijdelijk opslaan object en de gekoppelde relatie met een object één metaverse. Gekoppelde objecten worden gebruikt voor het synchroniseren van kenmerkwaarden tussen een verbindingslijn ruimte-object en een metaverse-object.

Wanneer een tijdelijk opslaan object een gekoppelde object tijdens de synchronisatie wordt, kunnen kenmerken tussen het tijdelijk opslaan object en het object metaverse doorlopen. Kenmerk stroom bidirectionele en is geconfigureerd met behulp van regels voor het kenmerk van importeren en exporteren kenmerk.

Een verbindingslijn één spatie object kan worden gekoppeld aan slechts één metaverse object. Elk object metaverse kan echter worden gekoppeld aan meerdere verbindingslijn ruimteobjecten in dezelfde of in andere verbindingslijn spaties, zoals wordt weergegeven in de volgende afbeelding.

![Arch5](./media/active-directory-aadconnectsync-understanding-architecture/arch5.png)

De gekoppelde relatie tussen het tijdelijk opslaan object en een object metaverse permanente is en kan worden verwijderd door regels die u opgeeft.

Een afzonderlijke object is een tijdelijk opslaan object dat niet is gekoppeld aan een willekeurig object metaverse. Het kenmerk waarden van een afzonderlijke object niet zijn verwerkt een verder binnen de metaverse. De waarden van het kenmerk van het bijbehorende object in de verbonden gegevensbron worden niet bijgewerkt door de synchronisatie-engine.

U kunt met behulp van afzonderlijke objecten, identiteit informatie opslaan in de synchronisatie-engine en deze later te verwerken. Een object tijdelijk opslaan als een afzonderlijke object in de ruimte verbindingslijn behouden heeft vele voordelen. Omdat het systeem de vereiste informatie over dit object al gefaseerde heeft, is het niet nodig is om te maken van een weergave van dit object opnieuw tijdens het volgende importeren uit de verbonden gegevensbron. Op deze manier synchronisatie-engine heeft altijd een volledige momentopname van de verbonden gegevensbron, zelfs als er momenteel geen verbinding met de verbonden gegevensbron. Afzonderlijke objecten kunnen worden omgezet in gekoppelde objecten en vice versa, afhankelijk van de regels die u opgeeft.

Een object importeren is als een afzonderlijke object gemaakt. Een object exporteren moet een gekoppelde object. De systeemlogica wordt deze regel wordt afgedwongen en verwijdert u elk object exporteren die niet een gekoppelde object.

## <a name="sync-engine-identity-management-process"></a>Synchronisatie-engine identiteit management proces
Het proces voor identiteit bepaalt hoe identiteit informatie tussen verschillende verbonden gegevensbronnen wordt bijgewerkt. Identiteitsbeheer voorkomt in drie processen:

- Importeren
- Synchronisatie
- Exporteren

Tijdens het importproces evalueert de synchronisatie-engine de binnenkomende identiteitsgegevens uit een gekoppelde gegevensbron. Wanneer wijzigingen worden gedetecteerd, deze maakt u nieuwe tijdelijk opslaan objecten of bestaande tijdelijk opslaan objecten in de ruimte connector voor synchronisatie wordt bijgewerkt.

Tijdens de synchronisatie van synchronisatie-engine de metaverse zodat wijzigingen die zijn opgetreden in de ruimte connector bijgewerkt en de ruimte verbindingslijn met wijzigingen die zijn opgetreden in de metaverse wordt bijgewerkt.

Tijdens het exportproces worden de synchronisatie-engine om de wijzigingen die zijn gefaseerd op testcomputers objecten en die zijn gemarkeerd als in behandeling exporteren.

De volgende afbeelding ziet u waar elk van de processen vindt plaats als identiteit informatiestromen uit één verbonden gegevensbron naar de andere.

![Arch6](./media/active-directory-aadconnectsync-understanding-architecture/arch6.png)

### <a name="import-process"></a>Importproces
Tijdens het importproces evalueert de synchronisatie-engine updates naar identiteit informatie. Synchronisatie-engine verschilt van de informatie van de identiteit hebt ontvangen van de verbonden gegevensbron met de identiteit informatie over een tijdelijk opslaan object en bepaalt of het tijdelijk opslaan object updates vereist. Zo nodig bij het tijdelijk opslaan object met nieuwe gegevens, wordt het tijdelijk opslaan object is gemarkeerd als in behandeling importeren.

Doordat objecten in de verbindingslijn spatie voor synchronisatie, kan de synchronisatie-engine alleen de identiteitsgegevens die zijn gewijzigd verwerken. Dit proces biedt de volgende voordelen:

- **Efficiënt synchronisatie**. De hoeveelheid gegevens die worden verwerkt tijdens de synchronisatie is geminimaliseerd.
- **Efficiënt het opnieuw**. U kunt wijzigen hoe synchronisatie-engine identiteit bepaalde informatie wordt verwerkt zonder opnieuw verbinding maken met de synchronisatie-engine met de gegevensbron.
- **Mogelijkheid om een voorbeeld van de synchronisatie te**. U kunt een voorbeeld bekijken van synchronisatie om te verifiëren of uw veronderstellingen over de identiteit management-proces juist zijn.

Voor elk object dat is opgegeven in de verbindingslijn, is de synchronisatie-engine eerst wordt gezocht naar een weergave van het object in de ruimte verbindingslijn van de verbindingslijn. Synchronisatie-engine wordt alle tijdelijk opslaan objecten in de ruimte verbindingslijn en probeert aan te melden een bijbehorende tijdelijk opslaan object die beschikt over een overeenkomende ankerkenmerk zoeken. Als u geen bestaande tijdelijk opslaan object een overeenkomende ankerkenmerk heeft, wordt synchronisatie-engine gezocht naar een overeenkomstige tijdelijk opslaan object met dezelfde DN-naam.

Wanneer de synchronisatie-engine vindt u een tijdelijk opslaan object dat overeenkomt met op DN-naam maar niet op anker, gebeurt het volgende speciale:

- Als het object in de ruimte verbindingslijn geen anker heeft, synchronisatie-engine dit object verwijdert uit de ruimte verbindingslijn en markeert u het metaverse-object dat deze is gekoppeld aan als **opnieuw inrichting bij volgende synchronisatie uitvoeren**. Vervolgens wordt het nieuwe importeren-object gemaakt.
- Als het object in de ruimte verbindingslijn een anker heeft, klikt u vervolgens synchronisatie-engine wordt ervan uitgegaan dat dit object is gewijzigd of verwijderd in de verbonden adreslijst. Een tijdelijke, nieuwe DN-naam voor het object van de ruimte verbindingslijn wilt toewijzen, zodat deze het binnenkomende object kunt voorbereiden. Het oude object wordt vervolgens **tijdelijke**, wachten op de verbindingslijn met de naam wijzigen of verwijderen voor het oplossen van de situatie importeren.

Als de synchronisatie-engine wordt gezocht naar een tijdelijk opslaan object dat overeenkomt met het object dat is opgegeven in de verbindingslijn, bepaalt welke wijzigingen wilt toepassen. Bijvoorbeeld: synchronisatie-engine mogelijk Wijzig of verwijder het object in de verbonden gegevensbron of deze mogelijk alleen de waarden van de kenmerken van het object bijwerken.

Tijdelijke objecten met bijgewerkte gegevens afwachting importeren. Verschillende soorten wachtende invoer zijn beschikbaar. Afhankelijk van het resultaat van het importproces heeft een tijdelijk opslaan object in de ruimte verbindingslijn een van de volgende handelingen uit in behandeling importeren typen:

- **Geen**. Geen wijzigingen in een van de kenmerken van het tijdelijk opslaan object zijn beschikbaar. Synchronisatie-engine niet vlag toevoegen dit type zoals wachtende importeren.
- **Toevoegen**. Het tijdelijk opslaan object is een nieuwe importeren object in de ruimte verbindingslijn. Synchronisatie-engine gemarkeerd dit type, zoals in behandeling importeren voor verdere verwerking in de metaverse.
- **Update**. Synchronisatie-engine vindt u een bijbehorende tijdelijk opslaan object in de ruimte verbindingslijn en dit type als wachtende importeren gemarkeerd, zodat updates voor de kenmerken die kunnen worden verwerkt in de metaverse. Updates bestaan object naam wijzigen.
- **Verwijderen**. Synchronisatie-engine vindt u een bijbehorende tijdelijk opslaan object in de ruimte verbindingslijn en dit type als wachtende importeren gemarkeerd, zodat het gekoppelde object kan worden verwijderd.
- **Delete/toevoegen**. Synchronisatie-engine vindt u een bijbehorende tijdelijk opslaan object in de ruimte verbindingslijn, maar de objecttypen komen niet overeen. In dit geval een verwijderen toevoegen wijziging is gefaseerde. A verwijderen-toevoegen wijziging wordt aangegeven dat de synchronisatie-engine dat een volledige opnieuw synchroniseren van dit object moet optreden omdat andere soorten regels op dit object toepassen wanneer het object typen wijzigingen.

De status in behandeling importeren van een tijdelijk opslaan object instelt, is het mogelijk te verminderen aanzienlijk de hoeveelheid gegevens die tijdens de synchronisatie is verwerkt, omdat dit kan dus het systeem verwerken alleen objecten die gegevens hebt bijgewerkt.

### <a name="synchronization-process"></a>Synchronisatieproces
Synchronisatie bestaat uit twee gerelateerde processen:

- Inkomende synchronisatie, als de inhoud van de metaverse wordt bijgewerkt met behulp van de gegevens in de ruimte verbindingslijn.
- Uitgaande synchronisatie, als de inhoud van de ruimte verbindingslijn wordt bijgewerkt met gegevens in de metaverse.

Met behulp van de gegevens in de ruimte verbindingslijn gefaseerde, de binnenkomende tijdens synchronisatie worden gemaakt in de metaverse de geïntegreerde weergave van de gegevens die zijn opgeslagen in de verbonden gegevensbronnen. Alle tijdelijk opslaan objecten of alleen die met een in behandeling importgegevens worden samengevoegd, afhankelijk van hoe de regels zijn geconfigureerd.

Het proces van uitgaande synchronisatie-updates exporteren objecten wanneer metaverse objecten wijzigen.

Binnenkomende synchronisatie Hiermee maakt u de geïntegreerde weergave in de metaverse identiteitsgegevens van de die is ontvangen van de verbonden gegevensbronnen. Synchronisatie-engine kunt identiteit informatie op elk gewenst moment verwerken met behulp van de meest recente informatie identiteit er uit de bron gekoppelde gegevens.

**Binnenkomende synchronisatie**

Binnenkomende synchronisatie bevat de volgende processen:

- **Inrichten** (ook wel **raming** als het is belangrijk om te onderscheiden van dit proces van het uitgaande synchronisatie inrichting). De synchronisatie-engine maakt een nieuw metaverse-object op basis van een object tijdelijk opslaan en deze koppelingen. Bepaling is een object op gebruikersniveau-bewerking.
- **Deelnemen aan**. De synchronisatie-engine koppelingen een tijdelijk opslaan object aan een bestaande metaverse-object. Een join is een object op gebruikersniveau-bewerking.
- **Importeren kenmerk stroom**. Synchronisatie-engine bijgewerkt de waarden van het kenmerk, kenmerk stroom, van het object in de metaverse genoemd. Importeren kenmerk stroom is een kenmerk niveau-bewerking die een koppeling tussen een tijdelijk opslaan object en een object metaverse nodig.

Bepaling is het enige proces die objecten in de metaverse maakt. Inrichten van invloed is op alleen objecten importeren die niet langer lid objecten zijn. Tijdens de bepalingen maakt de synchronisatie-engine een metaverse-object dat overeenkomt met het objecttype van het object importeren en tot stand brengt een koppeling tussen beide objecten, dus het maken van een gekoppelde object.

De join-proces wordt ook een koppeling tussen objecten importeren en een object metaverse. Het verschil tussen join en inrichten is dat de join-proces is vereist dat het object importeren zijn gekoppeld aan een bestaande metaverse-object, waar het proces bepaling maakt een nieuw metaverse-object.

Synchronisatie-engine wordt geprobeerd om een object importeren te koppelen aan een object metaverse met behulp van criteria die is opgegeven in de regel van de synchronisatie-configuratie.

Tijdens het inrichten en join, synchronisatie-engine een afzonderlijke object gekoppeld aan een object metaverse, zodat ze die zijn gekoppeld. Nadat u deze bewerkingen object op gebruikersniveau zijn voltooid, kunt synchronisatie-engine de waarden van het kenmerk van het object gekoppeld metaverse bijwerken. Dit proces wordt importeren kenmerk stroom genoemd.

Importeren kenmerk stroom doet zich voor op alle objecten van importeren die nieuwe gegevens bevatten en zijn gekoppeld aan een object metaverse.

**Uitgaande synchronisatie**

Uitgaande synchronisatie-updates exporteren objecten wanneer een object metaverse wijzigen, maar wordt niet verwijderd. Het doel van uitgaande synchronisatie is om te bepalen of wijzigingen in metaverse objecten updates voor het tijdelijk opslaan van de objecten in de spaties verbindingslijn vereisen. In sommige gevallen, de wijzigingen die tijdelijke kunnen vereisen objecten in alle verbindingslijn spaties worden bijgewerkt. Tijdelijke objecten die zijn gewijzigd worden gemarkeerd terwijl in behandeling exporteren, zodat ze objecten exporteren. Deze objecten later naar buiten wijzen met de verbonden gegevensbron tijdens het exportproces exporteren.

Uitgaande synchronisatie heeft drie processen:

- **Inrichten**
- **Deprovisioning**
- **Kenmerk stroom exporteren**

Inrichting en deprovisioning zijn beide bewerkingen object niveau. Deprovisioning, is afhankelijk van de inrichting van omdat alleen inrichting kunt starten. Deprovisioning wordt geactiveerd wanneer de koppeling tussen een metaverse-object en een object exporteren inrichting verwijdert.

Wijzigingen worden toegepast op objecten in de metaverse wordt altijd van geactiveerd op de inrichting van. Wanneer wijzigingen zijn aangebracht in metaverse objecten, kan een van de volgende taken uitvoeren als onderdeel van het inrichten synchronisatie-engine uitvoeren:

- Gekoppelde objecten, waar een metaverse-object is gekoppeld aan een nieuw gemaakte exporteren-object maken.
- De naam van een gekoppelde object.
- Meld koppelingen tussen een object metaverse en tijdelijke objecten maken van een afzonderlijke object.

Als de inrichting van vereist is voor synchronisatie-engine om een nieuwe verbindingslijn-object te maken, is het tijdelijk opslaan object waaraan u het object metaverse is gekoppeld altijd een object exporteren, omdat het object nog niet in de verbonden gegevensbron bestaat.

Als de inrichting is vereist voor synchronisatie-engine aan een gekoppelde object, maken van een afzonderlijke object, meld wordt deprovisioning geactiveerd. Het proces deprovisioning Hiermee verwijdert u het object.

Tijdens het deprovisioning verwijdert verwijderen van een object exporteren niet fysiek het object. Het object is gemarkeerd als **verwijderd**, wat betekent dat de verwijderbewerking is gefaseerde op het object.

Exporteren kenmerk stroom treedt ook op tijdens het uitgaande synchronisatie, net zoals dat importeren kenmerk flow tijdens de binnenkomende synchronisatie plaatsvindt. Exporteren kenmerk stroom doet zich alleen tussen metaverse en exporteren objecten die zijn gekoppeld.

### <a name="export-process"></a>Exportproces
Synchronisatie-engine worden tijdens het exportproces onderzocht, alle exporteren objecten die zijn gemarkeerd als in behandeling exporteren in de ruimte verbindingslijn en stuurt updates naar de verbonden gegevensbron.

De synchronisatie-engine kunt bepalen het succes van een exporteren maar deze niet voldoende bepalen dat de identiteit management voltooid is. Objecten in de verbonden gegevensbron kunnen altijd worden gewijzigd door andere processen. Omdat de synchronisatie-engine geen een permanente verbinding heeft met de verbonden gegevensbron, is het niet voldoende om ervoor te hypothesen over de eigenschappen van een object in de verbonden gegevensbron die is gebaseerd op een melding te exporteren.

Een proces in de verbonden gegevensbron kan bijvoorbeeld de kenmerken van het object wijzigen terug naar de oorspronkelijke instellingen (dat wil zeggen de verbonden gegevensbron, kunt u overschrijven de waarden direct nadat de gegevens worden door de synchronisatie-engine geactiveerd en in de verbonden gegevensbron toegepast).

De synchronisatie-engine winkels exporteren en importeren statusinformatie over elk object tijdelijk opslaan. Als de waarden van de kenmerken die zijn opgegeven in de kenmerklijst zijn gewijzigd sinds de laatste export, wordt de opslag van importeren en exporteren van status kunt synchronisatie-engine om te reageren op de juiste manier. Synchronisatie-engine wordt het importproces gebruikt om te bevestigen kenmerkwaarden die zijn geëxporteerd naar de verbonden gegevensbron. Een vergelijking tussen de geïmporteerde en geëxporteerde gegevens, kunt zoals wordt weergegeven in de volgende afbeelding, u synchronisatie-engine om te bepalen of de export is geslaagd of als deze moet worden herhaald.

![Arch7](./media/active-directory-aadconnectsync-understanding-architecture/arch7.png)

Bijvoorbeeld als de synchronisatie-engine Hiermee exporteert u kenmerk C, die een waarde van 5, met een verbonden gegevensbron heeft, wordt opgeslagen C = 5 in het geheugen van de status exporteren. Elke extra exporteren op de resultaten van dit object in een poging C = 5 wordt exporteren naar de verbonden gegevensbron opnieuw omdat de synchronisatie-engine wordt ervan uitgegaan dat deze waarde niet persistent op het object toegepast is (dat wil zeggen, tenzij een andere waarde is onlangs geïmporteerd uit de bron gekoppelde gegevens). Het geheugen exporteren wordt gewist wanneer C = 5 wordt ontvangen tijdens een importbewerking op het object.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de configuratie [Azure AD Connect synchroniseren](active-directory-aadconnectsync-whatis.md) .

Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).
