<properties
   pageTitle="Meer informatie over en een BizTalk regels API-app maakt in uw App logica | Microsoft Azure"
   description="In dit onderwerp behandelt de functies van BizTalk regels verbindingslijn en instructies voor het gebruik ervan"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="04/20/2016"
   ms.author="andalmia"/>

#<a name="biztalk-rules"></a>BizTalk regels

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Bedrijfsregels omvat de beleidsregels en besluiten waarmee wordt bepaald bedrijfsprocessen. Dit beleid mogelijk formeel velden worden gedefinieerd in procedure handleidingen, contracten of overeenkomsten, of als knowledge of expertise die in werknemers bestaat. Dit beleid zijn dynamisch en worden gewijzigd via afstemmen verschuldigd worden gewijzigd in bedrijfsplannen, regelgeving of andere reden.

Dit beleid implementeren in traditionele programmeertaal vereist veel tijd en afhankelijk en wordt niet-programmeurs deel te nemen aan maken en onderhouden van bedrijfsbeleid niet ingeschakeld. BizTalk bedrijfsregels biedt een manier om snel implementeren van dit beleid en loskoppelen van de rest van het bedrijfsproces. Hiermee vereiste wijzigingen aanbrengt in bedrijfsbeleid zonder die invloed hebben op de rest van het bedrijfsproces.

##<a name="why-rules"></a>Waarom regels

Er zijn 3 belangrijkste redenen om te gebruiken van bedrijfsregels BizTalk in bedrijfsproces:

* Loskoppelen van bedrijfslogica van toepassing-code
- Bedrijfsanalisten meer controle over bedrijven logica management toestaan
+ Wijzigingen in de bedrijfslogica Ga naar productie sneller

##<a name="rules-concepts"></a>Regels concepten

##<a name="vocabulary"></a>Woordenlijst

Een _woordenlijst_ is een verzameling definities bestaande uit een beschrijvende naam voor de gebruikte regelvoorwaarden en acties computing objecten. Woordenlijst definities makkelijker de regels te lezen, inzichtelijk maken en delen met personen in een bepaalde business-domein.

Woordenlijsten zijn ontworpen om de tussenruimte tussen zakelijke semantiek en implementatie-brug. Een gegevensverbinding voor een goedkeuringsstatus mogelijk bijvoorbeeld verwijzen naar een bepaalde kolom in een bepaalde rij in een bepaalde database, weergegeven als een SQL-query. In plaats van het invoegen van dit soort complexe weergave in een regel, kunt u in plaats daarvan de definitie van een woordenlijst, die is gekoppeld aan die gegevensbinding, met een beschrijvende naam "Status." maken Vervolgens kunt u 'Status' opnemen in een willekeurig aantal regels en de regel-engine kan de bijbehorende gegevens ophalen uit de tabel.

##<a name="rule"></a>Regel

Bedrijfsregels zijn declaratieve instructies voor het uitvoeren van bedrijfsprocessen. Een regel bestaat uit een voorwaarde en acties. De voorwaarde wordt geëvalueerd en als deze in waar resulteert, de regel-engine een of meer acties start.
Regels in het kader van de regels voor bedrijven worden gedefinieerd met behulp van de volgende indeling:

_IF_ _voorwaarde_ _Klik_ _actie_

Houd rekening met het volgende voorbeeld:

*Als hoeveelheid is kleiner dan of gelijk is aan de beschikbare financiële middelen*  
*VERVOLGENS leiden transactie en af te drukken ontvangstbevestiging*

Deze regel wordt bepaald of een transactie wordt uitgevoerd door de bedrijfslogica in de vorm van een vergelijking van twee monetaire waarden, in de vorm van een bedrag en de beschikbare financiële middelen toepassen.
U kunt de bedrijfsregel maken, wijzigen en implementeren van bedrijfsregels. U kunt ook de voorafgaande taken via programmacode uitvoeren.

###<a name="conditions"></a>Voorwaarden

Een voorwaarde is een waar/onwaar (Boole) expressie die uit een of meer predicaten bestaat.
In ons voorbeeld wordt het predikaatfunctie kleiner dan of gelijk aan toegepast op het bedrag en de beschikbare financiële middelen. Deze voorwaarde wordt altijd geëvalueerd als true of false.
Predicaten kunnen worden gecombineerd met de logische operatoren AND, OR en niet op het formulier een logische expressie die mogelijk erg groot is, maar wordt altijd geëvalueerd als true of false.

###<a name="actions"></a>Acties

Acties zijn de functionele gevolgen van de evaluatie van de voorwaarde. Als een voorwaarde is voldaan, zijn de bijbehorende actie of acties worden gestart.
In ons voorbeeld zijn "leiden transactie" en "ontvangstbewijs afdrukken" acties die worden uitgevoerd wanneer, en alleen als de voorwaarde (in dit geval "als hoeveelheid is kleiner dan of gelijk aan de beschikbare financiële middelen") waar is.
Acties worden aangegeven in het kader van de regels voor bedrijven met handelingen kunt verrichten instellen op XML-documenten.

##<a name="policy"></a>Beleid

Een beleid is een logische groepering van regels. U een beleid opstellen, opslaan, testen en, wanneer u tevreden met de resultaten bent, gebruikt u deze in een productieomgeving.

###<a name="policy-composition"></a>Beleid samenstelling

U kunt beleidsregels in de portal regels samenstellen. Een beleid kan een willekeurig grote set regels bevatten, maar meestal het opstellen van een beleid van regels die betrekking hebben op een specifiek zakelijk gebied binnen de context van de toepassing die van het beleid gebruikmaken.

###<a name="policy-testing"></a>Beleid testen
U kunt een test uitvoeren van uw beleid effectief uitvoeren voordat deze worden gebruikt in een productieomgeving. De Portal regels kunt u opgeven invoer voor een beleid, het beleid uitvoeren en de uitvoer te bekijken. De uitvoer bevat Logboeken, uitvoering van de regel, voorwaarde evaluatie, en de uitvoer van de resulterende.

##<a name="sample-scenario---insurance-claims"></a>Voorbeeldscenario - verzekeringen Claims
Laten we een voorbeeldscenario Volg te doorlopen deze als we de bedrijfslogica voor dezelfde opstellen.

![Alternatieve tekst][1]

In een scenario voor heel eenvoudig verzekeringsclaims indient hij zijn verzekeringen aanvraag (via een client zoals website, phone App, enzovoort). Deze Claim aanvragen wordt verzonden naar van het bedrijf claimen Processing per eenheid en op basis van het resultaat van de verwerking, het verzoek kan worden beide goedgekeurd, geweigerd of verzonden langs voor verdere handmatige verwerking.
De Claim Processing eenheid in ons scenario zou degene die de bedrijfslogica voor het systeem omvat. Het duurt deze eenheid beter wilt bekijken, ziet het volgende:

![Alternatieve tekst][2]

Nu laat ons bedrijfsregels gebruiken om deze bedrijfslogica te implementeren.


##<a name="creation-of-rules-api-app"></a>Het maken van regels Api-App


1. Meld u aan bij de Portal van Azure
2. Selecteer nieuwe -> Marketplace zoekt u naar *BizTalk regels*
3. Selecteer BizTalk regels in de lijst met resultaten. Hiermee opent u het blad BizTalk regels
4. Selecteer de knop *maken* ![alternatieve tekst][3]
1. Voer in het nieuwe blad dat wordt geopend, in de volgende informatie:  
    1. Naam: Geef een naam voor de regels API-App
    1. App-serviceplan – Selecteer of maak een nieuwe App-Service plannen
    1. Prijzen laag – de prijzen laag die u wilt dat deze App zich bevinden binnen kiezen
    1. Resourcegroep – Resourcegroep waarin de App moet zich bevinden in maken of selecteren
    2. Abonnement - Selecteer het abonnement dat u wilt gebruiken
    1. Plaats, namelijk de geografische locatie waar u de App om te worden geïmplementeerd zou nu kiezen.
4.  Selecteer *maken*. Binnen enkele minuten moet uw BizTalk regels API-App worden gemaakt.

##<a name="vocabulary-creation"></a>Woordenlijst maken
Na het maken van een BizTalk regels API-App, zou de volgende stap naar woordenlijsten maken. De verwachting is dat de ontwikkelaar de komt vaker persona om deze te voeren deze oefening. Als volgt hoe kom ik aan dit gedaan:


1. Uw BizTalk regels API App via de portal door te gaan naar bladeren starten -> API Apps - ><Your Rules API App>. Hiermee wordt krijgt u aan het Dashboard van regels API App vergelijkbaar met hieronder:

   ![Alternatieve tekst][4]

2. Selecteer "Woordenlijst definities". Hiermee wordt aangegeven met de woordenlijst Authoring scherm 3. Selecteer 'toevoegen' om te beginnen met het toevoegen van nieuwe woordenlijst definities.
Zijn er 2 typen woordenlijst definities momenteel ondersteund – letterlijke en XML.

##<a name="literal-definition"></a>Letterlijke definitie
1.  Nadat u hebt geklikt op 'Toevoegen', een nieuwe 'Definitie toevoegen' Blade wordt geopend. Voer de volgende waarden
  1.    Naam: alleen alfanumerieke tekens verwachting zonder speciale tekens. Dit is moet ook unieke aan de bestaande lijst met een woordenlijst-definitie.
  2.    Beschrijving: optionele veld.
  3.    Typ definitie, zijn er 2 typen die worden ondersteund. Kies letterlijke in dit voorbeeld
  4.    Gegevenstype – Hiermee kunnen gebruikers het gegevenstype van de definitie selecteren. Momenteel 4 gegevenstypen worden ondersteund: ik.  Tekenreeks – deze waarden moeten worden ingevoerd tussen dubbele aanhalingstekens ('voorbeeld tekenreeks")  
    II. Booleaanse – dit waar of ONWAAR kan zijn  
    III.    Nummeren – een decimaal getal  
    IV. DateTime-dit betekent dat de definition van het type datumtype. Gegevens moeten worden ingevoerd met deze opmaak – dd-mm-jjjj: mm: ss AM\PM  
  5. Input: dit is waar u de waarde van de definitie van uw invoeren. De waarden die u hier opgeeft moeten voldoen aan de door u gekozen gegevenstype. U kunt een enkele waarde, een verzameling waarden, gescheiden door komma's of een bereik van waarden met het trefwoord *naar*invoeren. U kunt bijvoorbeeld unieke waarde 1; invoeren een set 1, 2, 3; of een bereik van 1 tot en met 5. Merk op dat bereik wordt alleen ondersteund voor getallen.
  6. Selecteer *OK*.

![Alternatieve tekst][5]
##<a name="xml-definition"></a>XML-definitie
Als Type woordenlijst wordt geselecteerd als XML, de volgende invoer moet worden opgegeven  
  een.    Schema – klikken op deze wordt een nieuwe blade toestaan gebruiker om te kiezen uit een lijst met al geüploade schema's maken of uploaden van een nieuw mogen geopend.
b.    XPATH – deze invoer haalt ontgrendelen alleen na het kiezen van een schema in de vorige stap. Klikken op dit, verschijnt het schema dat is geselecteerd en kan de gebruiker selecteert u het knooppunt waarvoor de definitie van een woordenlijst moet worden gemaakt.  
  c.    FACULTEIT – deze invoer aangeeft welk gegevensobject zou worden ingevoerd in de regels-engine voor de verwerking. Dit is een geavanceerde eigenschappen en al dan niet standaard is ingesteld op het bovenliggende item van het geselecteerde XPATH. FACULTEIT wordt vooral voor koppelen en siteverzameling-scenario's.

![Alternatieve tekst][6]

### <a name="add-bulk"></a>Bulksgewijs toevoegen
De bovenstaande stappen hebt geregistreerd de ervaring voor het maken van begrippen definities. Zodra u hebt gemaakt, worden de definities gemaakte woordenlijst wordt weergegeven in lijstformulier. Zijn er vereisten kunnen meerdere definities van hetzelfde schema in plaats van de bovenstaande stappen opeenvolgende elke één keer te genereren. Dit is waar de mogelijkheid bulksgewijs toevoegen wordt bijzonder nuttig zijn.
Selecteren 'Bulksgewijs toevoegen', gaat u naar een nieuwe blade. De eerste stap is om te selecteren van het schema waarvoor meerdere definities zijn moet worden gemaakt. Als u deze wordt een nieuwe blade zodat u kunt een kiezen uit een lijst met al geüploade schema's dan toestaan voor het uploaden van een nieuwe record wordt geopend.
Nu krijgt de eigenschap XPath ontgrendeld. Als u deze wordt het Schema Viewer geopend waarin meerdere knooppunten kunnen worden geselecteerd.
De namen voor meerdere definities gemaakt standaard ingesteld op de naam van het knooppunt is geselecteerd. Deze kunnen altijd worden gewijzigd nadat de is gemaakt.

![Alternatieve tekst][7]

##<a name="policy-creation"></a>Beleid maken
Nadat de ontwikkelaar vereiste woordenlijsten gemaakt heeft, is de verwachting dat de bedrijfsanalist het één Business-beleid maken via de Portal Azure zou zijn.  
    1. Klik op de regels-App hebt gemaakt, moet u er een beleid lens te klikken op die de gebruiker wordt dieper op de pagina voor het maken van beleid is.  
    2. deze pagina wordt de lijst met beleidsregels voor die deze bepaalde regels App heeft weergegeven. De analisten kunt u een nieuw beleid toevoegen door gewoon een beleidsnaam te typen en kunt u door op tabblad tweemaal. Meerdere beleidsregels kunnen zich bevinden in een enkele regels API-App.  
    3. het gemaakte beleid selecteren wordt pas van de gebruiker aan de detailpagina van beleid waarin de regels die in het beleid kunnen zien.  
    ![Alternatieve tekst][8]
 4.  Selecteer 'Toevoegen' als u een nieuwe regel wilt toevoegen. Hiermee gaat u naar een nieuwe blade.

##<a name="rule-creation"></a>Regel maken
Een regel is een verzameling voorwaarde en actie-instructies. De acties worden uitgevoerd als de voorwaarde in waar resulteert. Geef een unieke naam (voor dat beleid) en regel beschrijving (optioneel) in het blad regel maken.
Het vak voorwaarde (IF) kan worden gebruikt om complexe voorwaardelijke overzichten te maken. Hier volgen de trefwoorden die worden ondersteund:  
1.  En – voorwaardelijke operator  
2.  Of – voorwaardelijke operator  
3.  bevat\_niet\_bestaat  
4.  bestaat  
5.  ONWAAR  
6.  is\_gelijk\_naar  
7.  is\_groter\_dan  
8.  is\_groter\_dan\_gelijk\_naar  
9.  is\_in  
10. is\_minder\_dan  
11. is\_minder\_dan\_gelijk\_naar  
12. is\_niet\_in  
13. is\_niet\_gelijk\_naar  
14. rest  
15. waar  

Het vak Action(THEN) bevatten meerdere instructies, één per regel, om te maken van de acties die moeten worden uitgevoerd. Hier volgen de trefwoorden die worden ondersteund:  
1.  is gelijk aan  
2.  ONWAAR  
3.  waar  
4.  stoppen  
5.  rest  
6.  Null  
7.  bijwerken  

De vakken voorwaarde en actie bieden Intellisense waarmee u snel een regel van de auteur. Dit kan worden veroorzaakt door kunt u door op ctrl + SPATIEBALK of door net begint te typen. Trefwoorden met getypte tekens worden gefilterd omlaag en weergegeven. De intellisense-venster wordt weergegeven voor alle trefwoorden en definities van de woordenlijst.
![Alternatieve tekst][9]

##<a name="explicit-forward-chaining"></a>Expliciete doorsturen koppelen
BizTalk regels ondersteunt expliciete koppelen, doorsturen als gebruikers wilt opnieuw evalueren regels in antwoord op bepaalde acties, ze kunnen zo activeren dat dit met behulp van bepaalde trefwoorden. Hier volgen de trefwoorden die worden ondersteund:  
   1.   bijwerken <vocabulary definition> – dit sleutelwoord opnieuw alle regels die de definitie van de opgegeven woordenlijst in de voorwaarde gebruiken moet worden geëvalueerd.  
   2.   Stoppen – dit sleutelwoord stopt alle regel executions

##<a name="enabledisable-rules"></a>Regels voor Enable\Disable
Elke regel in het beleid kan worden ingeschakeld of uitgeschakeld. Alle regels zijn al dan niet standaard ingeschakeld. Uitgeschakelde regels niet worden uitgevoerd tijdens de uitvoering van beleid. Regels voor Enable\Disable kunnen worden gedaan van het blad regel rechtstreeks – de opdrachten zijn beschikbaar in de opdrachtenbalk boven aan het blad of van het beleid, heeft het contextmenu (met de rechtermuisknop op een regel) de optie voor het enable\disable.

##<a name="rule-priority"></a>Regelprioriteit
Alle regels van een beleid worden uitgevoerd in volgorde. De prioriteit van uitvoering wordt bepaald door de volgorde waarin ze in het beleid voorkomen. Deze prioriteit kan worden gewijzigd door gewoon slepen en neerzetten van de regel.

##<a name="test-policy"></a>Beleid testen
U kunt uw beleid testen met behulp van de opdracht 'Beleid testen' in het blad beleid testen. In dit blade ziet u een lijst met woordenlijst definities die worden gebruikt in het beleid, de gebruikersinvoer van een nodig. Gebruikers kunnen handmatig toevoegen aan waarden voor deze ingangen voor hun Testscenario. U kunt ook gebruikers ook kunt importeren XMLs testen voor invoer. Als alle invoer in, de test kan worden uitgevoerd en de uitvoer van voor de definitie van elke woordenlijst wordt weergegeven in de uitvoerkolom voor eenvoudige vergelijking. Klik op 'Logboeken weergeven' om weer te geven van de logboeken worden uitgevoerd om weer te geven bedrijfsanalist beschrijvende Logboeken. Als u wilt de logboeken opslaan, de optie 'Output opslaan' is beschikbaar voor de opslag van alle gerelateerde gegevens voor een onafhankelijke analyse testen.

## <a name="using-rules-in-logic-apps"></a>Met behulp van regels in logica-Apps
Wanneer het beleid is geschreven en getest, is het nu gebruiksklare. U kunt een nieuwe logica-App maken door de logica Apps vanaf de linkerkant van de startpagina van de portal van kiezen. Zodra de logica-App is gemaakt, starten en selecteer vervolgens *Triggers en acties*. U kunt selecteert u de sjabloon *maken* . Volg de stappen voor het toevoegen van uw App BizTalk regels API aan de logica-App. Als dit is ingevuld, wordt er een optie om te kiezen welke regels API-App (actie) naar doelsite. Acties bevatten de lijst met beleidsregels die moeten worden uitgevoerd. Kies een specifieke beleid, waarna de invoer die zijn vereist voor het beleid moet worden opgegeven. Gebruikers kunnen de uitvoer van de voorbij regels API-App gebruiken voor verdere besluitvorming.

## <a name="using-rules-via-apis"></a>Met behulp van regels via API 's
De regels API-App kunnen ook worden gemaakt met een uitgebreide reeks API's. Deze manier waarop gebruikers worden niet beperkt tot werken alleen met logica-Apps en regels in een toepassing door REST gesprekken kunnen gebruiken. De exacte REST API beschikbaar kunnen worden bekeken door te klikken op de lens 'API definitie' in het dashboard regels.

![Alternatieve tekst][10]

Hierna volgt een voorbeeld van hoe een deze API in C gebruiken kan#

            // Constructing the JSON message to use in API call to Rules API App

            // xmlInstance is the XML message instance to be passed as input to our Policy
            string xmlInstance = "<ns0:Patient xmlns:ns0=\"http://tempuri.org/XMLSchema.xsd\">  <ns0:Name>Name_0</ns0:Name>  <ns0:Email>Email_0</ns0:Email>  <ns0:PatientID>PatientID_0</ns0:PatientID>  <ns0:Age>10.4</ns0:Age>  <ns0:Claim>    <ns0:ClaimDate>2012-05-31T13:20:00.000-05:00</ns0:ClaimDate>    <ns0:ClaimID>10</ns0:ClaimID>    <ns0:TreatmentID>12</ns0:TreatmentID>    <ns0:ClaimAmount>10000.0</ns0:ClaimAmount>    <ns0:ClaimStatus>ClaimStatus_0</ns0:ClaimStatus>    <ns0:ClaimStatusReason>ClaimStatusReason_0</ns0:ClaimStatusReason>  </ns0:Claim></ns0:Patient>";

            JObject input = new JObject();

            // The JSON object is to be of form {"<XMLSchemName>_<RootNodeName>":"<XML Instance String>".
            // In the below case, we are using XML Schema - "insruanceclaimsschema" and the root node is "Patient".
            // This is CASE SENSITIVE.
            input.Add("insuranceclaimschema_Patient", xmlInstance);
            string stringContent = JsonConvert.SerializeObject(input);


            // Making REST call to Rules API App
            HttpClient httpClient = new HttpClient();

            // The url is the Host URL of the Rules API App
            httpClient.BaseAddress = new Uri("https://rulesservice77492755b7b54c3f9e1df8ba0b065dc6.azurewebsites.net/");
            HttpContent httpContent = new StringContent(stringContent);
            httpContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json");

            // Invoking API "Execute" on policy "InsruranceClaimPolicy" and getting response JSON object. The url can be gotten from the API Definition Lens
            var postReponse = httpClient.PostAsync("api/Policies/InsuranceClaimPolicy?comp=Execute", httpContent).Result;

Houd er rekening mee dat de bovenstaande regels API-App beschikt over de beveiligingsinstellingen ingesteld op "Anon openbare". Dit kan worden ingesteld van binnen de API-App met: alle instellingen -> Toepassingsinstellingen -> toegangsniveau.

![Alternatieve tekst][11]

## <a name="editing-vocabulary-and-policy"></a>Woordenlijst en beleid bewerken
Een van de belangrijkste voordelen van het gebruik van bedrijfsregels is dat wijzigingen in de bedrijfslogica aan productie veel sneller kunnen worden geactiveerd. Elke wijziging begrippen en beleid wordt onmiddellijk toegepast in productie. Gebruiker hoeft om te bladeren naar de definitie van de desbetreffende woordenlijst of beleid en brengt u de wijziging zodat deze van kracht.

<!--Image references-->
[1]: ./media/app-service-logic-use-biztalk-rules/InsuranceScenario.PNG
[2]: ./media/app-service-logic-use-biztalk-rules/InsuranceBusinessLogic.png
[3]: ./media/app-service-logic-use-biztalk-rules/CreateRuleApiApp.png
[4]: ./media/app-service-logic-use-biztalk-rules/RulesDashboard.png
[5]: ./media/app-service-logic-use-biztalk-rules/LiteralVocab.PNG
[6]: ./media/app-service-logic-use-biztalk-rules/XMLVocab.PNG
[7]: ./media/app-service-logic-use-biztalk-rules/BulkAdd.PNG
[8]: ./media/app-service-logic-use-biztalk-rules/PolicyList.PNG
[9]: ./media/app-service-logic-use-biztalk-rules/RuleCreate.PNG
[10]: ./media/app-service-logic-use-biztalk-rules/APIDef.PNG
[11]: ./media/app-service-logic-use-biztalk-rules/PublicAnon.PNG
