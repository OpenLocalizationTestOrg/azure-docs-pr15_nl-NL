<properties 
    pageTitle="Handleiding Azure mobiele betrokkenheid aan de slag met aanbevolen procedures"
    description="Aan de slag-handleiding voor Azure Mobile betrokkenheid en aanbevolen procedures voor het onboarding" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="wesmc7777"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="10/04/2016"
    ms.author="wesmc;ricksal"/>

# <a name="azure-mobile-engagement---getting-started-guide-with-best-practices"></a>Azure mobiele betrokkenheid - handleiding aan de slag met aanbevolen procedures

## <a name="overview"></a>Overzicht

**Het mobiele scherm is een zeer vol spatie:** In 2013, heeft een studie gevonden dat het gemiddelde mobiele apparaat had 27 toepassingen hebt geïnstalleerd. Gebruikers die meestal 30 uren per maand aan hun apps besteed. De meeste ditmaal is besteed aan de sociale netwerken en gaming (ongeveer 20 uur). Vóór 2014 had Android market rond 1,5 miljoen toepassingen voor gebruikers van waaruit u kunt kiezen. De Apple store zich rond 1.2 miljoen apps. Gebruik van de mobiele app is nog steeds groter wordende zoals ontwikkelaars concurrentie in deze groeiende markt. 

De gemiddelde mobiele gebruiker wordt installeren en verwijderen van de apps gebruiken met hoge frequentie afhankelijk van het wijzigen van interesses en app ervaringen. Om te bepalen van het succes van een app wordt het belangrijke informatie meer dan ze alleen maar hoeveel gebruikers uw app installeren. Het is belangrijk om te weten hoe nuttig uw app wordt uitgevoerd en als die trend gebruik wijzigt. De volgende vragen worden belangrijke:

- Zijn uw gebruikers begin uw app uninteresting of verouderde vinden? 
- Hoeveel gebruikers zijn gestopt helemaal gebruiken van uw app? 
- App aankopen populair zijn omhoog of omlaag?
- Gebruikers lukt niet voltooid werk loopt vanwege problemen met de app of een gebrek aan rente? 
- Kan u behouden uw app nuttige en relevante door te drukken vers inhoud uw grondtal gebruiker? 
- Wilt u deze nieuwe inhoud niet hetzelfde zijn voor alle gebruikers of gericht op Gebruikersegmenten op basis van gedrag in uw app? 
 
Antwoorden op vragen die vergelijkbaar is met deze kunnen helpen de leven en inkomsten uit uw app uitbreiden. Ook kunt u problemen definiëren en uw base gebruiker bewaren. 

Media gerelateerde apps enkele van de hoogste bewaarbeleid onder gebruikers hebben meestal. Een reden hiervoor is dat ze worden voortdurend nieuwe inhoud leveren aan gebruikers. Vroege aanvaarding van handige push-meldingen doorgestuurd naar een bepaald gebruikerssegment vaak een hoge invloed hebben op app bewaarbeleid. 

Het programma Azure Mobile betrokkenheid is zo ontworpen dat u de leven en bewaren van uw app uitbreiden door op te geven van een methode voor het verzamelen en gedetailleerde informatie over het gebruik van de app te analyseren. Dit kunt u uw gebruikers op basis van de gedrag classificeren en focus campagnes voor het bezorgen van push-meldingen en berichten van de app aan geïdentificeerd Gebruikersegmenten te maken. Key performance indicators (KPI) meten hoe actieve gebruikers zijn met verschillende aspecten van de app. Azure Mobile betrokkenheid biedt de methoden die u moet deze KPI's bepalen. Is het handig het rendement van uw investering (ROI) vergroten via de infrastructuur die u nodig hebt om uit te breiden betrokkenheid met uw mobiele app. 

Wilt u optimaal Azure Mobile betrokkenheid, moet u beginnen met een plan goed ontworpen betrokkenheid. Uw abonnement kunt u de gedetailleerde gegevens die u mogelijk moet segmenten van het grondtal van de gebruiker bepalen. Dit kan worden gebaseerd op gedrag en app ervaringen. In de volgorde voor uw abonnement lukt, is een goede gewoonte om te definiëren duidelijk de KPI die de doelstellingen van de app meet. Met wissen prestaties indicatoren gedefinieerd, kunt u gemakkelijk de benodigde logica insluiten in uw app voor het verzamelen van prima korrelig gegevens die u gebruiken wilt voor het analyseren en evalueren van de KPI's. In dit onderwerp is een handleiding met aanbevolen procedures voor het definiëren van de KPI's die u met uw betrokkenheid-abonnement gebruiken wilt. 


## <a name="step-1-define-your-kpis-to-fit-the-bet-model"></a>Stap 1: De KPI's aanpassen aan het model relevante TREFFER definiëren


Correct definiëren KPI's zijn moeilijk om te voltooien. Apps die zijn ontworpen voor andere bedrijven hebben hun eigen specificaties en doelstellingen. Dit meestal mogelijk Verwar de methode. Om te voorkomen dat dit, doelstellingen en KPI's moeten worden ingedeeld in drie belangrijkste categorieën: **bedrijven**, **betrokkenheid**en **technische**. Dit is wat verwijst naar de **relevante TREFFER model**.

Een goed plan heeft over het algemeen doelstellingen met de KPI's die de uitkomsten in elk van de volgende categorieën van het model relevante TREFFER meten.


#### <a name="business-kpis"></a>KPI's voor bedrijven

Zakelijke KPI's is de eenvoudigste onderdeel maken. U waarschijnlijk al gedefinieerd deze in een vorm wanneer u uw mobiele app hebt gepland. Deze KPI's helpen in het algemeen maateenheid voor opbrengst en ROI voor u app. De volgende lijst bevat enkele voorbeelden van bedrijven KPI's waarmee u helpen bij het definiëren van uw prestaties indicatoren mogelijk:

- Media bedrijven KPI 's
    - Aantal advertenties hebt geklikt
    - Aantal pagina bezoeken per gebruiker
    - Aantal huidige abonnementen
- Spellen bedrijven KPI 's
    - Aantal app aankopen
    - Gemiddelde omzet per gebruiker (ARPU)
    - Tijd per sessie
    - Dagen afgespeeld en huidige in spel niveau
- E-Commerce bedrijven KPI 's
    - Dagen app gebruikt
    - Gemiddelde omzet per gebruiker (ARPU)
    - Gemiddelde bedrag in winkelwagen afrekenen.
    - Productcategorie voor de meeste weergaven en aankopen
- Bank en verzekeringen bedrijven KPI 's
    - Aantal accounts
    - Functies die worden geactiveerd
    - Aanbieding pagina's bezocht
    - Waarschuwingen geklikt of geactiveerd      



#### <a name="engagement-kpis"></a>Betrokkenheid KPI 's

Een KPI betrokkenheid is een performance indicator meten de betrokkenheid van uw gebruikers. Trends in dit gedeelte vast te stellen van het bewaarbeleid van de app. Hier volgen een paar voorbeeld prestatie-indicatoren voor dit type KPI:

- Actieve gebruikers in de laatste zeven dagen
- Aantal niet-actieve gebruikers voor de laatste zeven dagen
- Het aantal gebruikers die de app niet hebt gebruikt in 30 dagen  

Enkele duidelijke externe factoren invloed kunnen hebben op indicatoren in dit gedeelte. Bijvoorbeeld, kunt u overwegen een mobiel apparaat te allen tijde met een gebruiker. Dit al dan niet waar. Hogere gebruik rond feestdagen wanneer een gamer meer terwijl niet op het werk- of uitzoomen op school speelt hebben mogelijk een app spellen meestal.   

Ook gedefinieerd kunt KPI's in deze categorie u de relatie tussen uw app en uw klanten meten.



#### <a name="technical-kpis"></a>Technische KPI 's

Performance Indicator in deze categorie kunnen u bepalen of uw app is goed, verkeerd-om of vast. Deze indicatoren kunnen de status van uw app meten en bruikbaarheid problemen die kunnen verhinderen dat gebruikers met de app bepalen. Gegevens die worden verzameld voor deze categorie kan ook informatie over de prestaties die relevant zijn voor marketing teams bevatten. De gegevens kan ook worden problemen oplossen IT en ondersteuning voor teams om vast te stellen van niet-aangegeven bugs bij te werken. 
 
Hier volgen enkele voorbeelden van technische KPI's:

- Informatie over onverwerkte of verwerkte uitzonderingen en aantal 
- Tijdstempel voor vastlopen
- Laatste geklikt of laatste pagina bezocht
- Geheugengebruik van de app
- Tarief van de App-frame
- Versie van het besturingssysteem dat de app wordt uitgevoerd op
- App-versie

Deze KPI's als u wilt meten app wilt verbeteren en mogelijke fouten pinpoint definiëren. Deze indicatoren moeten verminderen, de tijd die u nodig hebt voor het leveren van een oplossing voor uw klanten. Ze kunnen ook helpen u bij het definiëren van een bepaald gebruikerssegment die een bepaalde problemen ondervindt. U kunt deze gebruikerssegmentering maken van campagnes om meldingen over oplossingen beschikbaar en mogelijke promoties klanttevredenheid herstellen. 


#### <a name="playbook-exercise-1-create-your-kpi-dashboard"></a>Playbook Oefening 1: Uw dashboard KPI maken

Wanneer u uw strategie voor marketingactiviteit definieert, moeten een weergave in de KPI's presenteren voor elk van uw belangrijkste doelstellingen. Ze moeten duidelijk omschreven gegevenspunten waarmee u voor het verzamelen van belangrijke informatie als u wilt controleren van uw app en het gedrag van de eindgebruiker.

Maken van een KPI-dashboard met de onderstaande informatie

1.  Wat zijn de KPI's voor de app?
2.  Welke gegevenspunten kan ik gebruiken om aan te geven van de KPI's?
3.  Waar kan ik deze gegevens voor de toepassing (dat wil zeggen scherm, instellingen, systeem...) vinden?
4.  Kan ik een reeks betrokkenheid voor deze KPI afspelen?

U kunt de **Opbouwfunctie voor KPI** -werkblad in onze [Media Playbook sjabloon] [ Media Playbook link] voor voorbeelden en richtlijnen.



## <a name="step-2-your-engagement-program"></a>Stap 2: Uw betrokkenheid-programma


Een geweldige mobiele betrokkenheid programma moet worden beschouwd als een belangrijk onderdeel van uw app. Dit dient absoluut een geweldige Welkom programma die wordt uitgevoerd voor een gebruiker in de eerste dagen van appgebruik omvatten. Dit vaak een zeer positief gevolgen hebben voor betrokkenheid en het bewaarbeleid van de app. Onderzoek hebt weergegeven dat de meeste gebruikers stoppen met een app de eerste paar dagen na de installatie. Wilt u streven om te voldoen aan of klant verwachtingen groot rente vroeg overschrijdt terwijl de gebruiker nog steeds is gericht op uw app. Zorg ervoor dat u de sleutelwaarde en voordelen van uw app presenteren aan uw klanten. 


![](./media/mobile-engagement-getting-started-best-practices/unsegmented-push-notifications.png)

Push-meldingen zijn de beste manier voor vroege afspraken met gebruikers van mobiele apparaten. Echter uitstekende voorzichtig wanneer gebruikers voor push-meldingen segmenteren. Omdat wanneer een gebruiker iets ze wel ontvangen spam of uninteresting meldingen, kan dit ernstige invloed hebben. Een gebruiker kan uw toepassing nooit om terug te keren verwijderen paar muisklikken. De gebruiker moet ten zeerste gepersonaliseerde app-waarde in plaats van de algemene spam ontvangt.

Als gebruikers actief betrokken blijft zijn, kan klikt u vervolgens het programma betrokkenheid helpen station andere aspecten van de app.

U kunt een campagne waarin uw actieve gebruikers de app waarderen bijvoorbeeld kan instellen. Aangezien dit gebruikerssegment het meest actief is en de meeste ervaring met u app heeft, kunt u ze wilt geven van de meest nauwkeurige classificatie zou verwachten. Nadat u een beoordeling hoog app hebt, kunt deze station van het organisch downloaden van de app ook verkleinen van uw nieuwe aanschafkosten van de klant.



#### <a name="engagement-sequence"></a>Betrokkenheid sequentie


Een globale betrokkenheid programma bevat verschillende betrokkenheid reeksen. Elke reeks probeert te bereiken van verschillende doelstellingen.


###### <a name="life-push-sequence"></a>Leven push-sequentie


De doelstellingen voor een reeks van de push leven verschillen afhankelijk van de levenscyclus van de betrokkenheid van de gebruiker met de app. Een bepaalde gebruiker mogelijk nieuwe, inactief of zeer actieve. Gebruikers kunnen op verschillende fasen van een levenscyclus profiteren van uw nieuwe inhoud in de vorm van tips of koppelingen voor informatie over. 

Een nieuwe gebruiker kan bijvoorbeeld nodig ophalen gericht op een app-help of profiteren van een nieuwe gebruiker zodat extra aantrekkelijk ongeveer als volgt uit de eerste keer dat ze de app start...

*"Graag hebt u boord! Vergeet niet aanmelden bij het ophalen van de 1e maand gratis."*


###### <a name="behavioral-push-sequence"></a>Doorgevoerd push-sequentie

De volgorde doorgevoerd push ernaar gebruik op basis van de gebruikersgedrag die worden verzameld voor de app te verhogen.  

Een zeer actieve gebruiker van een fictieve kampioenschap-app nuttig bijvoorbeeld met de volgende push-melding te verrichten...

*"John u bent een ventilator ernstige kampioenschap! Meld u aan bij de sectie van onze NFL en gratis toegang tot de SuperBowl winnen!'*


###### <a name="alerting-push-sequence"></a>Waarschuwingen push-sequentie

Gebruikers waarderen relevant nieuws gericht op hun interesses. Een waarschuwing push-reeks verbetert de betrokkenheid door het verzenden van waarschuwingen op basis van interesses die een gebruiker heeft duidelijk weergegeven. Dit kan zijn expliciete wanneer een gebruiker hun eigen interesses in de app selecteert. Dit kan ook worden bepaald impliciet op basis van gegevens die tijdens de interactie van de gebruiker met de app verzameld.

De gebruiker van een E-Commerce-app kan bijvoorbeeld regelmatig kopen voor een specifieke huisstijl koffie die u hebt vastgelegd met een bedrijf KPI. De volgende melding kunt verbeteren door de betrokkenheid van deze gebruiker met de app.
 
*"Hallo Wes, wordt een van uw favoriete merken koffie worden op verkoop 25% uit de eerste week van September 2015. Waarderen we u als een klant en het gewenste type om te controleren of u zich bewust was."*

###### <a name="rentention-push-sequence"></a>Rentention push-sequentie

Deze reeks ernaar gebruikers met behulp van een terugkerende push-meldingen campagnes station een gewone gewoonte van aantrekkelijke met de app bewaren. Dit kan helpen app bewaarbeleid verhogen als de gebruiker de interacties bezit. 

Bijvoorbeeld: de gebruiker van een gerelateerde Sport-app mogelijk het volgende bericht push wekelijkse op basis van favoriete teams van de gebruiker:

*"Voor een mogelijkheid om winnen 200 punten, gaat u stem of de Yankees New York wordt winnen hun spel deze week tegen Amsterdam blauw Jays!"*


#### <a name="the-3w-approach"></a>De methode 3W

De verschillende push-reeksen mastering, kunt u oefenen met eindgebruikers. Echter nog steeds moet u de methode 3W gebruiken voor het personaliseren van uw berichten. De methode 3W kan worden opgelost die, waar en wanneer voor elk bericht. Als u duidelijk voldoen aan deze drie vragen moet u meldingen goed voor betrokkenheid gericht.

![](./media/mobile-engagement-getting-started-best-practices/who-what-when.png)



###### <a name="who-the-user-segment-that-will-receive-messages"></a>Wie: De gebruiker segment dat berichten ontvangt

Zet nieuwe meldingen aan uw gebruikers moet worden beschouwd als een zeer gevoelige communicatiekanaal. Controleer of de meldingen die u wilt verzenden naar een bepaald gebruikerssegment gericht ook zijn beperkt tot het belang van die gebruikerssegment. Er is een onjuist routering melding zeer kunnen een negatieve invloed hebben op een gebruiker. Ze overwegen deze spam waardoor uw app wordt verwijderd. 

Gebruik een combinatie van specifieke technische en doorgevoerd criteria bij het definiëren van de gebruiker de lijnsegmenten die meldingen ontvangt. Een eenvoudig voorbeeld definiëren van een bepaald gebruikerssegment kan er ongeveer als volgt:

"Alle gebruikers die gestart de een mobiele toepassing voor de eerste keer 3 dagen geleden en de aanmeldingspagina tweemaal zonder het uitvoeren van een aanmelding hebt bezocht '.
 
Deze instructie kunt u de gegevens die u verzamelen moet voor een specifieke scenario identificeren.


###### <a name="what-the-message-that-you-will-send"></a>Wat: Het bericht dat u verzendt

**Toon**

Gebruik in uw afspraken een toon die geschikt is voor uw voor uw gesegmenteerde gebruikers. Dit is groeiende een goede manier om te communiceren met uw eindgebruikers en een gebruikerswachtwoord rente in uw app verhogen. 

**Omleiding**

Een push-bericht kan worden gebruikt voor het openen van meer dan van de toepassing. Als het meldingsbericht een context zoals broadcast nieuws of een product promotie, deze melding biedt mogelijk uitgebreide koppeling rechtstreeks naar de juiste inhoud binnen de toepassing. Daarom moet u een URL-schema laat de toepassing de omleiding beheren. Wanneer u werkt aan uw reeksen betrokkenheid, is dit een belangrijke stap die u moet niet vergeten.

Omleiding kan ook worden beheerd voor andere systemen. Bijvoorbeeld, met een actie-URL is het mogelijk om te leiden eindgebruikers vele andere systemen waaronder de volgende:

- Een website
- Een Postvak met e-mail al ingesteld
- Een SMS-vak
- Een externe service
- Rechtstreeks naar de store toepassing voor beoordeling van de toepassing. 

Dit biedt vele mogelijkheden om deelnemen eindgebruikers en automatische regels voor het verbeteren van de prestaties te maken.


**Opmaak/inhoud**

Verschillende typen en de Push notification-indelingen:

1. **Aankondigingen** : kunt u reclame berichten verzenden naar gebruikers op verschillende momenten (afmelden bij de app in de app of altijd).
2. **Polls** : u kunt gegevens verzamelen van eindgebruikers door ze vragen ingeschakeld. Deze antwoorden zijn vervolgens beschikbaar bij het maken van criteria voor eindgebruikers doel.
3. **Gegevens worden** : kunt u een binair getal of base64-gegevensbestand naar de app bijwerken verzenden. De gegevens in een push gegevens is verzonden naar uw persoonlijke voorkeur aanpassen de gebruikers in uw app-toepassing. Uw toepassing moet worden ter ondersteuning van de gegevens in een push gegevens.
4. **Tegels (alleen voor Windows Phone)** : helpen bij het gebruik van de Microsoft Push-meldingen Service (MPNS) om te verzenden systeemeigen Push-meldingen met XML-gegevens (ondersteunde sinds SDK versie 0.9.0. De uiteindelijke nettolading voor tegels niet langer zijn dan 32 kB.). Het bericht wordt weergegeven rechtstreeks op de tegel van uw bord.
5. **Webweergave** : een webweergave is een pop-met webinhoud. In dit pop-upvenster wordt weergegeven wanneer de eindgebruiker heeft op de push-meldingen hebt geklikt. Een webweergave kunt u meer interactie met de eindgebruiker.
 
>[AZURE.NOTE] Controleren of de inhoud die u als push-meldingen verzendt overeenstemt met het desbetreffende platform (iOS, Android, Windows) richtlijnen voor het ontwikkelen van apps en het verzenden van push-meldingen.

 


###### <a name="when-the-timing-of-your-campaign"></a>Wanneer: De timing van de campagne

Wanneer is de beste tijd om het activeren van een campagne push-meldingen activeert? Mag niet handmatig of automatisch? Moet deze terugkerend? Bepalen van de juiste tijd of frequentie is essentiële kunt voeren gebruikers met de beste resultaten. Voor elke betrokkenheid sequentie en scenario, moet u opgeven wanneer is de beste tijd om het verzenden van push-meldingen. Hier volgen enkele mogelijke voorbeelden:

![](./media/mobile-engagement-getting-started-best-practices/campaign-timing-examples.png)

Als u veel meldingen dagelijks verzendt, moet u ernstige aandacht uitvoeren dat uw gebruikers uw communicatie als spam kunnen zien. 

Azure Mobile betrokkenheid zijn er twee manieren om te helpen uw communicatie wordt beschouwd als spam voorkomen. Gebruik eerst gedetailleerde segmentatie zodat u dezelfde gebruikers niet doen afstemmen. Daarnaast biedt Azure Mobile betrokkenheid een quotafunctie "". Deze functie kunt meldingen verzonden voor een campagne beperken. Bijvoorbeeld een standaardquotum instelt op 5 per week zorgt ervoor dat die een gebruiker als onderdeel van de campagne gebruikerssegment niet meer dan 5 meldingen voor die week ontvangt opgenomen.





#### <a name="playbook-exercise-2-create-your-engagement-program"></a>Playbook Oefening 2: Uw programma betrokkenheid maken

Wat uitgebreider samenvatten uw doelstellingen en de campagnes die u verwacht om af te spelen met behulp van specifieke reeksen definiëren. Zorg ervoor dat u de methode 3W toepassen op de meldingen in uw campagnes. 

Het werkblad **Betrokkenheid programma** gebruiken in de [Media Playbook sjabloon] [ Media Playbook link] voor voorbeelden en richtlijnen.


## <a name="step-3-app-integration"></a>Stap 3: App-integratie


#### <a name="create-a-tag-plan"></a>Maak een plan tag

Azure Mobile betrokkenheid integreren met uw app moet u een tag plan opstellen. Het label-abonnement is de hoeksteen van het project. Hiermee definieert u de relatie tussen de marketingactiviteit specificaties, de werkstroom van de toepassing en de reële tag-gegevens die worden verzameld in de app meten KPI's. Geeft aan welke analytics is mogelijk om te zien in de portal. Ook kunt u Gebruikersegmenten definiëren en focus push-meldingen kunt voeren uw eindgebruikers verzenden. Eenmaal u de tag-abonnement hebt gedefinieerd, de code wilt integreren met uw app is eenvoudig met de SDK van Azure Mobile betrokkenheid toevoegen.

Een tag-abonnement moet niet Alles markeren in een toepassing. Deze mag alleen tag-gegevens die deel uitmaakt van uw mobiele betrokkenheid strategie bevatten. Dit waarschijnlijk diverse tussen toepassingen. De [Media Playbook sjabloon] [ Media Playbook link] mits door Azure Mobile betrokkenheid Hiermee kunt u een tag-abonnement kunt met een bepaalde methode maken. De **Tag abonnement** werkblad als een handleiding voor het samenstellen van uw abonnement tag gebruiken.

Wanneer u een tag sectie definieert in het werkblad, worden zeer specifieke. Dit is heel belangrijk om verwarring te voorkomen. Elk detail verwacht scenario waarbij elk naamkaartje wordt verzonden. De naam van de activiteit waarbij elk naamkaartje is ingesloten bevatten. Dit moet al worden opgenomen in het gedeelte **Informative** van het werkblad. Het werkblad van het label moet de belangrijkste referentie voor test-verificatie. 

In de sectie **gegevens worden verzameld** , moet het ontwikkelteam typen, namen, waarden en extra info sleutel/waardeparen vereist voor elk naamkaartje die worden ingesloten in de toepassing zoeken.

Het is raadzaam om het abonnement Tag bekijken met alle teams die is gekoppeld aan het project. Benodigde correcties aanbrengen en controleren of dat Alles wissen voor marketing- en ontwikkelteams is.

Het werkblad **overzicht van werkzaamheden** kan worden gebruikt om te werken met die iedereen bij het project betrokken.


#### <a name="data-types"></a>Gegevenstypen

Dit zijn de algemene typen gegevensondersteuning door Azure Mobile betrokkenheid.

###### <a name="devices-and-users"></a>Apparaten en gebruikers

Azure Mobile betrokkenheid geeft aan wat gebruikers door te genereren van een unieke id voor elk apparaat. Deze id heet het apparaat-id (of deviceid). Zodanig dat alle toepassingen op hetzelfde apparaat dezelfde apparaat-id delen wordt gegenereerd.

###### <a name="sessions-and-activities"></a>Sessies en activiteiten

Een sessie wordt een exemplaar van de app wordt uitgevoerd door een gebruiker. De sessie zich uitstrekt over vanaf het moment dat de gebruiker de app wordt gestart totdat wordt stopgezet.

Een activiteit is een logische groepering van een reeks wat die de app tijdens een sessie doen kan. Het is gewoonlijk een bepaald scherm in de app, maar mogelijk dat iets gedefinieerd door de logica van de toepassing. Minimaal moet u elke scherm of activiteit labelen voor de app. Hierdoor kunt u voor meer informatie over het gebruiker-pad.


###### <a name="events"></a>Gebeurtenissen

Gebeurtenissen worden gebruikt om de interactie van de gebruiker met de app. Dit steekt nogal direct acties, zoals delen van inhoud of starten van een video. Gebeurtenissen labelen, krijgt u met het verzamelen van gegevens die zien hoe gebruikers werken met de app. 

###### <a name="jobs"></a>Taken

Taken worden gebruikt om acties die een duur hebben. Enkele voorbeelden zou doen:

- Uitvoering van API-oproepen
- Weergavetijd van advertenties
- Achtergrond taken duur 
- Aankoop-proces duur
- Een video bekijken


###### <a name="errors"></a>Fouten

Fouten worden gebruikt om problemen met de app gedetecteerd. Bijvoorbeeld, onjuiste gebruikersacties of API gesprek fouten.

###### <a name="application-information"></a>Informatie over toepassingen

Toepassingsinformatie (App-Info) wordt gebruikt voor het markeren van gegevens die zijn gerelateerd aan de ervaring van een gebruiker met een toepassing. Het wordt gegenereerd door de interactie met de toepassing. 

Voor de sleutel van een bepaalde app-info, Azure Mobile betrokkenheid alleen houdt van de laatste waarde (geen geschiedenis). App-info klikt, worden de status van uw app of uw eindgebruikers. Bijvoorbeeld de status aanmelden, of van een gebruiker favoriete productgroep.

###### <a name="crash-data"></a>Vastlopen van gegevens

Gegevens automatisch die worden verzameld door de mobiele betrokkenheid SDK rapporten toepassingsfouten niet verwerkt door de toepassing vastlopen. Bijvoorbeeld een onverwerkte uitzondering die optreedt.


###### <a name="extra-data"></a>Extra gegevens

Gebeurtenissen, fouten, activiteiten en taken worden uitgebreid met parameters. Dit is een ontwikkelaar als specifieke gegevens van de toepassing mogelijk bieden extra-informatie. Dit is belangrijk is voor het definiëren van fijnmazige Segmentatie. 

Bijvoorbeeld de waarde van een label 'artikel', kunt u naar het segment eindgebruikers op basis van die specifieke bepaling bekeken. Echter die mogelijk niet genoeg. Het mogelijk beter als deze tag wordt dezelfde 'artikel' gebruikt ook extra-info zoals "news_category" binnen een activiteit opgenomen. Dit is handig om te bepalen dynamisch de favoriete categorieën voor de gebruiker. 

Extra info wordt vermeld als een paar sleutelwaarde /. In het voorbeeld van deze mediatoepassing zou de extra-informatie voor 'news_category' de waarde voor die categorie. Bijvoorbeeld "sports", "economie" of "politiek".





#### <a name="tag-and-sdk-integration"></a>Integratie van de tag en SDK 

Voor stapsgewijze instructies voor het integreren van de Azure Mobile betrokkenheid SDK in uw app, volgt u de documentatie [Betrokkenheid SDK integratie](mobile-engagement-windows-store-integrate-engagement.md) op de website van de Azure. Kies uw doelplatform van de koppelingen boven aan deze pagina.

Het is raadzaam om projecten voor twee gebouwd betrokkenheid van Azure Mobile-apps te maken. Een handtekening voor ontwikkelen en test tijdelijke en de andere voor productie tijdelijke. Uw IT-team kunt vervolgens uit test tijdelijke promoveren naar productie wanneer de gebruiker acceptatie testen geslaagd is.



#### <a name="user-acceptance-testing-uat"></a>Gebruiker acceptatie (UAT) — testen

Gebruiker acceptatie testen (UAT) — heeft betrekking op ervoor te zorgen dat alles werkt zoals ontworpen. Processen kunnen worden voltooid en verzamel alle vereiste gegevens op basis van uw abonnement tag:
 
- Informatie labelen moet op de plaats op basis van de beschreven AZME concepten
- Alle gegevens worden verzameld (inclusief Extra info waarde, App info waarde)
- Nomenclatuur overeenkomsten op basis van de deling Tag plannen
- Er is geen dubbele labels verzonden

Alle typen meldingsgedrag die u hebt ingesloten in uw app uitvoerig te testen

- Aankondigingen, Polls, gegevens worden afmelden bij de app en in-app
- Tekst/Web weergaven
- Badge update, categorieën



#### <a name="setup"></a>Setup

Instellen van Azure Mobile betrokkenheid is heel eenvoudig. De documentatie die zijn gerelateerd aan de gebruikersinterface is beschikbaar op de website van Azure Mobile betrokkenheid [het navigeren in de gebruikersinterface](mobile-engagement-user-interface-home.md).

Het wordt aanbevolen dat u door het instellen van de juiste rollen en rollidmaatschappen voor de gebruikers van uw project begint. Hiermee kunt u de juiste toegang tot het platform voor alle gebruikers beheren. Uw rollen kunnen bestaan uit:

- Beheerders
- Ontwikkelaars
- Viewers 

Achteraf:
- Uw deviceID om te testen op uw eigen apparaat registreren.
- Ga naar de instellingen van uw account en de tijdzone instellen om te laten grafieken en het moment van bezorging melding instellen voor uw tijdzone.
- Ga naar de instellingen van uw toepassing en het 'App-info' u binnen handbereik doel eindgebruikers moet registreren.

Bekijk [hoe u aan de slag gebruiken en gereedschap duwt bereiken af om uw eindgebruikers beheren](mobile-engagement-how-tos.md)voor meer informatie over het uitvoeren van de eerste push-meldingen campagne.



## <a name="conclusion"></a>Sluiten


Programma's betrokkenheid iteratieve zijn en u moet vergeten continu verbeteren als u experimenteren met wat het beste voor uw app werkt. 

In eerste instantie tijdens het ontwikkelen van ervaring met betrokkenheid strategieën niet probeert te maken van een hele globale betrokkenheid strategie. Een stap voor stap aanpak identificeren de KPI's en hoe u gebruikmaken van deze duren. Betrokkenheid strategie wordt voor elke app uniek zijn.

Nadat u de enige ervaring hebt ontwikkeld kunt u overwegen de volgende manieren te werk toevoegen aan uw betrokkenheid-programma's:

- Bijhouden: Opgehaalde gebruikers en u waarschijnlijk gegevensverzameling bronnen definiëren. Azure mobiele betrokkenheid kan worden gekoppeld aan gegevensverzameling bronnen. Dit kunt u om de prestaties van elke bron te houden. Deze informatie is interessant voor de investering acquisition maximaliseren. 

- A / B testen: dit is een essentieel onderdeel van betrokkenheid programma. Elke app heeft een eigen manier. Met A / B testen, kunt u het programma betrokkenheid verbeteren.

- Geografische locatie: Dit is een grote kans voor merken. U kunt bereiken met behulp van deze functie op de juiste plaats verzonden en de tijd. Het is raadzaam bevestigt dat u voldoende eindgebruikers gedrag gegevens hebt verzameld voordat u begint met het gebruik van de geografische locatie.

- Push van gegevens: gegevens push wordt een onzichtbare push. Gegevens push kunt aanpassen van uw toepassing op basis van eindgebruikers werking. Als een bepaald gebruikerssegment raadpleegt vaak high tech-producten, kan de eigenaar van de app bijvoorbeeld een push gegevens die haar welkomstpagina, met Geavanceerd inhoud wordt personaliseren verzenden.



## <a name="next-steps"></a>Volgende stappen

- [Een mobiele betrokkenheid van Azure-account maken](mobile-engagement-create.md).
- Ga naar [uw strategie Mobile betrokkenheid definiëren](mobile-engagement-define-your-mobile-engagement-strategy.md) voor meer informatie over het definiëren van uw strategie Mobile betrokkenheid.



  

<!--Image references-->


<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
