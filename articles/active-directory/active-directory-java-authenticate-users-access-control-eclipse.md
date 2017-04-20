<properties
    pageTitle="Het gebruik van toegangsbeheer (Java) | Microsoft Azure"
    description="Informatie over het ontwikkelen en beheren in Access gebruiken met Java in Azure wordt aangegeven."
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm" />

# <a name="how-to-authenticate-web-users-with-azure-access-control-service-using-eclipse"></a>Hoe webgebruikers worden geverifieerd met Azure Access Control-Service Eclips gebruiken

Deze handleiding leert u hoe u de Azure Access Control Service (ACS) binnen de Toolkit Azure voor Eclips gebruikt. Zie voor meer informatie over ACS, de sectie van de [volgende stappen](#next_steps) .

> [AZURE.NOTE]
> Het Filter Azure Access Services besturingselement is een voorbeeld van de technologie community. Als bètasoftware, wordt deze niet formeel ondersteund door Microsoft.

## <a name="what-is-acs"></a>Wat is ACS?

De meeste ontwikkelaars zijn niet identiteit experts en over het algemeen niet wilt besteden tijd ontwikkelen verificatie en machtiging regelingen voor hun toepassingen en services. ACS is een Azure-service die u een eenvoudige manier gebruikers die moeten toegang hebben tot uw webtoepassingen en services zonder te hoeven factor complexe verificatie-logica in uw code te verifiëren.

De volgende functies zijn beschikbaar in ACS:

-   Integratie met Windows identiteit Foundation (WIF).
-   Ondersteuning voor populaire web-identiteitsprovider (IP-adressen) zoals Windows Live ID, Google, Yahoo! en Facebook.
-   Ondersteuning voor Active Directory Federation Services (AD FS) 2.0.
-   Een Open Data Protocol (OData)-management-service die via programmacode toegang naar ACS-instellingen biedt.
-   Een beheerportal waarmee beheerderstoegang tot de ACS-instellingen.

Zie voor meer informatie over ACS, [Access Control Service 2.0][].

## <a name="concepts"></a>Concepten

Azure ACS is gebaseerd op de principes op claims gebaseerde identiteit - een consistente manier voor het maken van verificatiemethoden voor on-premises implementatie-toepassingen of in de cloud. Op claims gebaseerde identiteit biedt een gebruikelijke manier voor toepassingen en services om de identiteit benodigde informatie over gebruikers binnen hun organisatie, in andere organisaties en klik op Internet.

Als u wilt de taken in deze handleiding uitvoeren, moet u de volgende basisbegrippen uitvoeren:

**Client** - In de context van deze Procedurebeschrijving, dit is een browser die probeert toegang te krijgen tot uw webtoepassing.

**Toepassing van de fabrikant (RP) Relying** - toepassing een RP is een website of de service die verificatie aan één externe instantie heeft. In de identiteit vaktermen dat we dat de RP die instantie vertrouwt. Deze handleiding wordt uitgelegd hoe u uw toepassing configureren om te vertrouwen ACS.

**Token** - een token is een verzameling beveiligingsgegevens dat meestal na een geslaagde verificatie van een gebruiker is uitgegeven. Een reeks *claims*, kenmerken van de geverifieerde gebruiker bevat. Een claim kan de naam van een gebruiker vertegenwoordigen, een id voor een gebruiker van een rol behoort, van een gebruiker leeftijd, enzovoort. Een token wordt meestal digitaal ondertekend, wat betekent dit kan altijd worden die afkomstig zijn terug naar de uitgever en de inhoud kan worden geknoeid. Een gebruiker krijgt toegang tot een toepassing RP door een geldig token uitgegeven door een instantie die de toepassing RP vertrouwensrelaties presenteren.

**Identiteit Provider (IP)** - een IP is een instantie die wordt geverifieerd door gebruikersidentiteit en beveiligingstokens. De werkelijke hoeveelheid werk van tokens worden uitgeven wordt geïmplementeerd Hoewel een speciale service beveiliging Token Service (STS) genoemd. Normale voorbeelden van IP-adressen zijn Windows Live ID, Facebook, zakelijke gebruiker opslagplaatsen (zoals Active Directory), enzovoort.
Wanneer ACS is geconfigureerd om te vertrouwen een IP, wordt het systeem accepteren en tokens uitgegeven door die IP valideren. ACS kunt meerdere IP-adressen in één keer, wat betekent dat wanneer uw toepassing ACS vertrouwt, u direct de toepassing op alle geverifieerde gebruikers alle IP-adressen bieden kunt dat ACS vertrouwensrelaties namens vertrouwen.

**Federatie Provider (inch)** - IP-adressen directe kennis van gebruikers, en ze met hun referenties verifiëren en claims over wat ze over deze weten actie. Een Federatie Provider (inch) is een ander soort instantie: in plaats van rechtstreeks verifiëren van gebruikers, het fungeert als een tussenpersoon en makelaars verificatie tussen één RP en een of meer IP-adressen. Zowel IP-adressen en FPs beveiligingstokens, zodat ze beide beveiliging Token Services (STS) gebruiken. ACS is één inch.

**Regel-Engine ACS** - de logica voor binnenkomende tokens afgeleid van vertrouwde IP-adressen naar tokens deel uit moeten worden gebruikt door de RP Transformeren is in de vorm van een eenvoudige claims transformatie regels gaan. ACS functies van een regel-engine die zorgt voor het toepassen van ongeacht transformatie logica die u hebt opgegeven voor uw RP.

**ACS Namespace** - een naamruimte is een partition hoogste niveau van ACS dat u gebruikt voor het ordenen van uw instellingen. Een naamruimte bevat een lijst met IP-adressen u vertrouwt, de RP-toepassingen die u wilt gebruiken, de regels dat u verwacht dat de regel te verwerken binnenkomende tokens met de engine, enzovoort. Een naamruimte beschrijft de verschillende eindpunten die wordt gebruikt door de toepassing en de ontwikkelaar om ACS om uit te voeren van de functie.

De volgende afbeelding ziet hoe ACS-verificatie werkt met een webtoepassing:

![ACS gegevensstroom-diagram][acs_flow]

1.  De client (in dit geval een browser) vraagt een pagina van de RP.
2.  Aangezien het verzoek is nog niet geverifieerd, wordt de RP de gebruiker naar de instantie die vertrouwde, welke ACS is. De ACS biedt de gebruiker de keuze van IP-adressen die zijn opgegeven voor dit RP. De gebruiker selecteert het juiste IP.
3.  De client naar de pagina verificatie van het IP-bladert en een bericht dat de gebruiker aan te melden.
4.  Nadat de client is geverifieerd (bijvoorbeeld de identiteit referenties zijn ingevoerd), wordt in de IP een beveiligingstoken problemen.
5.  Na het verzenden van een beveiligingstoken, het IP doorverwezen naar ACS en de client stuurt het beveiligingstoken uitgegeven door de IP naar ACS.
6.  ACS is gevalideerd met het beveiligingstoken uitgegeven door de IP, invoeritems de identiteit vorderingen in deze token in de ACS regels-engine, de uitvoer identiteitsclaims berekent en een nieuwe beveiligingstoken dat deze claims uitvoer bevat problemen.
7.  ACS doorverwezen naar de RP. De client verzendt het nieuwe beveiligingstoken dat is uitgegeven door ACS naar de RP. De RP is gevalideerd met de handtekening in het beveiligingstoken uitgegeven door ACS, de claims in deze token is gevalideerd en geeft als resultaat de pagina die oorspronkelijk is aangevraagd.

## <a name="prerequisites"></a>Vereisten voor

Als u wilt de taken in deze handleiding uitvoeren, moet u het volgende:

- Een Java ontwikkelaars Kit (JDK), v 1,6 of hoger.
- IDE Eclips voor Java EE ontwikkelaars, Indigo of hoger. Dit kan worden gedownload van <http://www.eclipse.org/downloads/>. 
- Een verdeling van een webserver Java gebaseerde of toepassingsserver, zoals Apache Tomcat, GlassFish, JBoss-toepassingsserver of Jetty.
- een Azure-abonnement die kan worden aangeschaft van <http://www.microsoft.com/windowsazure/offers/>.
- Los van de Azure Toolkit voor Eclips, April 2014 of hoger. Zie de [installatie van de Azure Toolkit voor Eclips](http://msdn.microsoft.com/library/windowsazure/hh690946.aspx)voor meer informatie.
- Een x.509-certificaat voor gebruik met uw toepassing. Moet u dit certificaat in zowel openbare certificaat (.cer) en Personal Information Exchange (. PFX)-indeling. (Opties voor het maken van dit certificaat worden beschreven verderop in deze zelfstudie).
- Bekend zijn met de Azure berekenen emulator en implementatie-technieken die bij het [maken van een Hallo wereld-toepassing voor Azure in Eclips](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx)worden besproken.

## <a name="create-an-acs-namespace"></a>Een Namespace ACS maken

Als u wilt beginnen met het gebruik van Access Control Service (ACS) in Azure, moet u een naamruimte ACS maken. De naamruimte biedt een unieke bereik voor de adressering van ACS resources uit binnen de toepassing.

1. Meld u aan bij de [Portal van Azure Management][].
2. Klik op **Active Directory**. 
3. Als u wilt een nieuwe toegangsbeheer naamruimte maken, klikt u op **Nieuw**op **App-Services**, klikt u op **Toegangsbeheer**, en klik vervolgens op **Snelle maken**. 
4. Voer een naam voor de naamruimte. Azure wordt gecontroleerd of de naam uniek is.
5. Selecteer het gebied waarin de naamruimte wordt gebruikt. Gebruik het gebied waarin u uw toepassing implementeert voor de beste prestaties.
6. Als u meer dan één abonnement hebt, selecteert u het abonnement waaraan u wilt gebruiken voor de naamruimte ACS.
7. Klik op **maken**.

Azure wordt gemaakt en de naamruimte activeert. Wacht totdat de status van de nieuwe naamruimte **actief** voordat u verdergaat is. 

## <a name="add-identity-providers"></a>Identiteitsprovider toevoegen

In deze taak voegt u IP-adressen voor gebruik met uw RP-toepassing voor verificatie. Batchbestand, deze taak ziet u hoe u Windows Live toevoegen als een IP-, maar u kunt een van de IP-adressen in de beheerportal ACS vermeld.


1.  Klik in de [Beheerportal van Azure][] **Active Directory**op, selecteer een naamruimte beheren in Access en klik vervolgens op **beheren**. De beheerportal ACS wordt geopend.
2.  Klik in het linkernavigatiedeelvenster van de beheerportal ACS, klikt u op **identiteitsprovider**.
3.  Windows Live ID is standaard ingeschakeld en kan niet worden verwijderd. Voor toepassing van deze zelfstudie wordt wordt alleen Windows Live ID gebruikt. Dit scherm is echter waar u andere IP-adressen, kan toevoegen door te klikken op de knop **toevoegen** .

Windows Live ID is nu een IP-voor de naamruimte ACS ingeschakeld. Vervolgens u uw Java-webtoepassing (om te worden gemaakt later) opgeven als een RP.

## <a name="add-a-relying-party-application"></a>De toepassing van een gebruikmakende leverancier toevoegen

In deze taak configureert u ACS herkend uw Java-webtoepassing als een geldige RP-toepassing.

1.  Klik in de beheerportal ACS op **toepassingen van derden Relying**.
2.  Klik op de pagina **Toepassingen van derden te vertrouwen** , klikt u op **toevoegen**.
3.  Ga als volgt te werk op de pagina **Te vertrouwen partijen-toepassing toevoegen** :
    1.  Typ in het vak **naam**de naam van de RP. Typ voor toepassing van deze zelfstudie wordt **Azure Web App**.
    2.  Selecteer in de **modus** **Instellingen invoeren handmatig**.
    3.  Typ in **Realm**, de URI waarop het beveiligingstoken uitgegeven door ACS van toepassing is. Voor deze taak, typt u **http://localhost:8080 /**.
        ![Feest-domein voor gebruik in berekeningscluster emulator vertrouwen][relying_party_realm_emulator]
    4.  Typ de URL waaraan ACS geeft als het beveiligingstoken resultaat in **Retour-URL** . Voor deze taak, typt u **http://localhost:8080/MyACSHelloWorld/index.jsp**
        ![Relying partijen retour-URL voor gebruik in berekeningscluster emulator][relying_party_return_url_emulator]
    5.  Accepteer de standaardwaarden in de rest van de velden.

4.  Klik op **Opslaan**.

U hebt nu geconfigureerd uw webtoepassing Java wanneer deze wordt uitgevoerd in de emulator Azure berekeningscluster (bij http://localhost:8080 /) moeten een RP in uw naamruimte ACS. Maak vervolgens de regels die ACS gebruikt om te verwerken van vorderingen die voortvloeien uit het RP.

## <a name="create-rules"></a>Regels maken

In deze taak definieert u de regels die aansturen hoe claims van IP-adressen naar uw RP worden doorgestuurd. Uitgaande van dit voorbeeld wordt we gewoon ACS als u wilt kopiëren van de invoer claimtypen en waarden rechtstreeks in het token uitvoer zonder filteren of wijzigen van deze bestanden configureren.

1.  Klik op de hoofdpagina ACS Management Portal, op **groepen met regels**.
2.  Klik op **Groep op de regel standaard voor Azure Web App**op de pagina **Groepen met regels** .
3.  Klik op **genereren**op de pagina **Groep van regel bewerken** .
4.  Klik op de **regels genereren: regel groep standaard voor Azure Web App** pagina, zorgen Windows Live ID is ingeschakeld en klik op **genereren**.    
5.  Klik op de pagina **Regel groep bewerken** op **Opslaan**.

## <a name="upload-a-certificate-to-your-acs-namespace"></a>Een certificaat uploaden naar uw ACS naamruimte

In deze taak die u uploadt een. PFX-certificaat dat wordt gebruikt token aanvragen die door uw naamruimte ACS gemaakt zich aan te melden.

1.  Klik op de hoofdpagina ACS Management Portal, op **certificaten en sleutels**.
2.  Klik op **toevoegen** boven **Token-ondertekening**op de pagina **certificaten en sleutels** .
3.  Klik op de pagina **certificaat toevoegen Token-ondertekening of de sleutel** :
    1. In het gedeelte **gebruikt voor** **Feest-toepassing te vertrouwen** op en selecteer **Azure Web App** (die u eerder hebt ingesteld als de naam van uw gebruikmakende partij-toepassing).
    2. Selecteer in de sectie **Type** **X.509-certificaat**.
    3. Klik op de bladerknop en Ga naar het x.509-certificaat-bestand dat u wilt gebruiken in de sectie **certificaat** . Dit is een. PFX-bestand. Selecteer het bestand op **openen**en typ het certificaatwachtwoord in het vak **wachtwoord** . Houd er rekening mee dat een zelfstandige-signed-certificaat voor testdoeleinden kan gebruiken. Als u wilt een zelfondertekend certificaat maken, gebruikt u de knop **Nieuw** in het dialoogvenster **ACS Filter bibliotheek** is (later beschreven) of gebruik het hulpprogramma **encutil.exe** uit de [project-website][] van de Azure Starter Kit voor Java.
    4. Zorg ervoor dat **U op primair** is ingeschakeld. De pagina **certificaat toevoegen Token-ondertekening of toets** ziet er ongeveer als volgt uit.
        ![Certificaat voor token-ondertekening toevoegen][add_token_signing_cert]
    5. Klik op **Opslaan** om uw instellingen opslaan en sluiten van de pagina **certificaat toevoegen Token-ondertekening of -toets** .

Vervolgens Controleer de gegevens op de pagina integratie van toepassingen en kopieer de URI die u nodig hebt voor het configureren van uw webtoepassing Java ACS gebruiken.

## <a name="review-the-application-integration-page"></a>Bekijk de pagina met de integratie van toepassingen

U vindt alle gegevens en de code die nodig zijn voor het configureren van uw webtoepassing Java (de RP toepassing) voor gebruik met ACS op de pagina toepassingsintegratie van de ACS-beheerportal. Moet u deze informatie tijdens het configureren van uw Java-webtoepassing voor federatieve verificatie.

1.  Klik in de beheerportal ACS op **integratie van toepassingen**.  
2.  Klik op de **Aanmeldingspagina's** **Integratie van toepassing** op de pagina.
3.  Klik op de pagina **Integratie van de pagina Login** op **Azure Web App**.

In de **integratie van de pagina Login: Azure Web App** pagina, de URL in **optie 1: koppeling naar een gehoste ACS aanmeldingspagina** wordt gebruikt in uw Java-webtoepassing. Moet u deze waarde wanneer u de bibliotheek Azure toegang besturingselement Services Filter aan uw Java-toepassing toevoegt.

## <a name="create-a-java-web-application"></a>Een webtoepassing Java maken
1. Binnen Eclips, op het menu klikt u op **bestand**, klikt u op **Nieuw**en klik vervolgens op **Dynamische Web Project**. (Als er geen **Dynamische Web Project** vermeld als een beschikbare project na te klikken op **bestand**, **Nieuw**en voer daarna de onderstaande stappen: klik op **bestand**, klikt u op **Nieuw**, klik op **Project**, **Web**uitvouwen, klik op **Dynamische Web Project**en klik op **volgende**.) Geef het project **MyACSHelloWorld**voor toepassing van deze zelfstudie wordt. (Zorgen dat u deze naam gebruiken, opeenvolgende stappen in deze zelfstudie verwachten uw WAR-bestand MyACSHelloWorld naam). Uw scherm ziet er ongeveer als volgt uit:

    ![Een project Hallo allemaal voor ACS exampple maken][create_acs_hello_world]

    Klik op **Voltooien**.
2. Uitvouwen **MyACSHelloWorld**binnen de Eclips Projectverkenner weergave. Met de rechtermuisknop op **webinhoud**, klikt u op **Nieuw**en klik vervolgens op **JSP-bestand**.
3. Klik in het dialoogvenster **Nieuw JSP-bestand** een naam geven het bestand **index.jsp**. Houd de bovenliggende map als MyACSHelloWorld/webinhoud, zoals wordt weergegeven in het volgende:

    ![Een bestand JSP ACS bijvoorbeeld toevoegen][add_jsp_file_acs]

    Klik op **volgende**.

4. Selecteer **Nieuwe JSP-bestand (html)** in het dialoogvenster **JSP sjabloon selecteren** en op **Voltooien**.
5. Wanneer het index.jsp-bestand is geopend in Eclips toevoegen in weer te geven tekst **Hallo ACS wereld!** binnen de bestaande `<body>` element. Uw bijgewerkte `<body>` inhoud moet worden weergegeven als volgt te werk:

        <body>
          <b><% out.println("Hello ACS World!"); %></b>
        </body>
    
    Sla index.jsp.
  
## <a name="add-the-acs-filter-library-to-your-application"></a>De bibliotheek ACS Filter toevoegen aan uw toepassing

1. In de Eclips Projectverkenner met de rechtermuisknop op **MyACSHelloWorld**op **Pad maken**en klik vervolgens op **Maken pad configureren**.
2. Klik op het tabblad **bibliotheken** in het dialoogvenster **Java bouwen pad** .
3. Klik op **bibliotheek toevoegen**.
4. Klik op **Azure toegang besturingselement Services filteren (op MS openen Tech)** en klik op **volgende**. Het dialoogvenster **Azure toegang besturingselement Services Filter** wordt weergegeven.  (Het veld **locatie** mogelijk een ander pad, afhankelijk van waar u Eclips, geïnstalleerd en het versienummer kan afwijken, afhankelijk van de software-updates.)

    ![ACS Filter bibliotheek toevoegen][add_acs_filter_lib]

5. Gebruik van een browser geopend naar de pagina **Login pagina integratie** van de beheerportal, Kopieer de URL die wordt vermeld de **optie 1: koppeling naar een gehoste ACS aanmeldingspagina** veld en plak deze in het veld **ACS verificatie eindpunt** van het dialoogvenster Eclips.
6. Met een browser geopend naar de pagina **Te vertrouwen partijen-toepassing bewerken** van de beheerportal, Kopieer de URL in het veld **Realm** wordt vermeld en plak deze in het veld **Te vertrouwen partijen Realm** van het dialoogvenster Eclips.
7. In de sectie **beveiliging** van het dialoogvenster Eclips als u wilt een bestaand certificaat te gebruiken, klikt u op **Bladeren**, Ga naar het certificaat dat u wilt gebruiken, selecteert u deze en klik op **openen**. Of, als u wilt maken van een nieuw certificaat, klikt u op **Nieuw** om het dialoogvenster **Nieuw certificaat** weer te geven, geef het wachtwoord, de naam van het .cer-bestand en de naam van de .pfx-bestand voor het nieuwe certificaat.
8. Controleer **het certificaat in het WAR-bestand insluiten**. Het insluiten van het certificaat op deze manier bevat deze in uw implementatie zonder dat u deze handmatig toevoegen als een onderdeel. (Als u moet uw certificaat in plaats daarvan extern opslaan vanuit uw WAR-bestand, u kan het certificaat toevoegen als een onderdeel van de rol en schakel **het certificaat in het WAR-bestand insluiten**.)
9. [Optionele] Behouden **vereisen HTTPS-verbindingen** ingeschakeld. Als u deze optie instelt, moet u toegang tot uw-toepassing via het HTTPS-protocol. Als u niet dat vereisen HTTPS-verbindingen wilt, schakelt u deze optie uit.
10. De filterinstellingen **Azure ACS** ziet er ongeveer als volgt uit voor een implementatie naar de emulator berekeningscluster.

    ![Azure ACS filterinstellingen voor een implementatie naar de emulator berekenen][add_acs_filter_lib_emulator]

11. Klik op **Voltooien**.
12. Klik op **Ja** wanneer wordt aangeboden met een dialoogvenster waarin wordt vermeld dat een web.xml-bestand wordt gemaakt.
13. Klik op **OK** om het dialoogvenster **Java bouwen pad** te sluiten.

## <a name="deploy-to-the-compute-emulator"></a>Implementeren naar de emulator berekenen

1. In de Eclips Projectverkenner met de rechtermuisknop op **MyACSHelloWorld** **Azure**op en klik vervolgens op **Inpakken voor Azure**.
2. Voor **de naam van het Project**, typ **MyAzureACSProject** en klik op **volgende**.
3. Selecteer een JDK en toepassingsserver. (Deze stappen worden beschreven in detail in deze zelfstudie [maken van een Hallo wereld-toepassing voor Azure in Eclips](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) ).
4. Klik op **Voltooien**.
5. Klik op de knop **uitvoeren in Azure Emulator** .
6. Nadat uw webtoepassing Java wordt gestart in de emulator berekeningscluster, sluit u alle exemplaren van uw browser (zodat alle huidige browsersessies uw ACS login test niet storen).
7. Voer uw toepassing door te openen <> http://localhost:8080/MyACSHelloWorld/in uw browser ( <> of/https://localhost:8080/MyACSHelloWorld/als u **vereisen HTTPS-verbindingen**ingeschakeld). U moet worden gevraagd om een Windows Live ID-aanmelding en klik vervolgens u moet worden genomen om de retour-URL voor uw gebruikmakende partij-toepassing opgegeven.
99.  Wanneer u klaar bent met het weergeven van uw toepassing, klik op de knop **Azure Emulator opnieuw instellen** .

## <a name="deploy-to-azure"></a>Implementeren naar Azure

Als u wilt implementeren naar Azure, moet u het gebruikmakende partij domein wijzigen en terug te keren van URL voor de naamruimte ACS.

1. Wijzigen **Realm** om de URL van uw site geïmplementeerd in de beheerportal Azure, op de pagina **Te vertrouwen partijen-toepassing bewerken** . **Voorbeeld** vervangen door de DNS-naam die u hebt opgegeven voor uw implementatie.

    ![Feest-domein voor gebruik in productie vertrouwen][relying_party_realm_production]

2. Wijzig de **Retour-URL** de URL van uw toepassing. **Voorbeeld** vervangen door de DNS-naam die u hebt opgegeven voor uw implementatie.

    ![Te vertrouwen partijen retour-URL voor gebruik in productie][relying_party_return_url_production]

3. Klik op **Opslaan** om op te slaan uw bijgewerkte beantwoorden partijen realm en URL wijzigingen terug te keren.
4. Houd de **Integratie van de pagina Login** -pagina in uw browser is geopend, moet u voor het kopiëren van deze binnenkort.
5. In de Eclips Projectverkenner met de rechtermuisknop op **MyACSHelloWorld**op **Pad maken**en klik vervolgens op **Maken pad configureren**.
6. Klik op het tabblad **bibliotheken** , klikt u op **Azure toegang besturingselement Services Filter**en klik vervolgens op **bewerken**.
7. Gebruik van een browser geopend naar de pagina **Login pagina integratie** van de beheerportal, Kopieer de URL die wordt vermeld de **optie 1: koppeling naar een gehoste ACS aanmeldingspagina** veld en plak deze in het veld **ACS verificatie eindpunt** van het dialoogvenster Eclips.
8. Met een browser geopend naar de pagina **Te vertrouwen partijen-toepassing bewerken** van de beheerportal, Kopieer de URL in het veld **Realm** wordt vermeld en plak deze in het veld **Te vertrouwen partijen Realm** van het dialoogvenster Eclips.
9. In de sectie **beveiliging** van het dialoogvenster Eclips als u wilt een bestaand certificaat te gebruiken, klikt u op **Bladeren**, Ga naar het certificaat dat u wilt gebruiken, selecteert u deze en klik op **openen**. Of, als u wilt maken van een nieuw certificaat, klikt u op **Nieuw** om het dialoogvenster **Nieuw certificaat** weer te geven, geef het wachtwoord, de naam van het .cer-bestand en de naam van de .pfx-bestand voor het nieuwe certificaat.
10. Behouden **insluiten het certificaat in het bestand WAR** ingeschakeld, ervan uitgaande dat u wilt het certificaat in het WAR-bestand insluiten.
11. [Optionele] Behouden **vereisen HTTPS-verbindingen** ingeschakeld. Als u deze optie instelt, moet u toegang tot uw-toepassing via het HTTPS-protocol. Als u niet dat vereisen HTTPS-verbindingen wilt, schakelt u deze optie uit.
12. Voor een implementatie naar Azure, ziet de filterinstellingen Azure ACS er ongeveer als volgt uit.

    ![Azure ACS filterinstellingen voor een productie-implementatie][add_acs_filter_lib_production]

13. Klik op **Voltooien** om het dialoogvenster **Bibliotheek bewerken** te sluiten.
14. Klik op **OK** om te sluiten van het dialoogvenster **Eigenschappen voor MyACSHelloWorld** .
15. Klik in Eclips, op de knop **publiceren op Azure Cloud** . Reageer op de aanwijzingen, vergelijkbare als voltooid in de sectie **naar uw Azure-toepassing implementeren** van het onderwerp [een Hallo wereld-toepassing voor Azure in Eclips maken](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) . 

Nadat uw webtoepassing is geïmplementeerd, sluit u eventuele geopende browsersessies, uw webtoepassing uitvoeren en wordt u gevraagd het aan te melden met Windows Live ID-referenties, gevolgd door naar de retour-URL van uw gebruikmakende partij-toepassing is verzonden.

Als u klaar bent met uw toepassing ACS Hallo allemaal onthouden verwijderen van de implementatie (u kunt lezen hoe u het verwijderen van een implementatie in het onderwerp [een Hallo wereld-toepassing voor Azure in Eclips maken](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) ).


## <a name="next_steps"></a>Volgende stappen

Zie voor een onderzoek van de beveiliging bevestiging Markup Language (SAML) geretourneerd door ACS in uw toepassing, [het weergeven van SAML die het resultaat van de Azure-Service voor het beheer van Access][]. Verder te verkennen van ACS functionaliteit en om te experimenteren met meer geavanceerde scenario's, Zie [Access besturingselement Service 2.0][].

In dit voorbeeld worden ook de optie **insluiten het certificaat in het bestand WAR** gebruikt. Deze optie kunt u eenvoudig implementeren van het certificaat. Als u in plaats daarvan blijven uw handtekeningcertificaat gescheiden van uw WAR-bestand wilt, kunt u de volgende methode gebruiken:

1. Typ in de sectie **beveiliging** van het dialoogvenster **Azure toegang besturingselement Services Filter** **${envelop. JAVA_HOME}/mycert.cer** en schakelt u **het certificaat in het WAR-bestand insluiten**uit. (Mijncert.cer aanpassen de bestandsnaam van het certificaat wordt weergegeven). Klik op **Voltooien** om het dialoogvenster te sluiten.
2. Kopieer het certificaat als onderdeel van uw implementatie: Projectverkenner In Eclips **MyAzureACSProject**uitvouwen, met de rechtermuisknop op **WorkerRole1**, klikt u op **Eigenschappen**, **Azure rol**uitvouwen en op **onderdelen**.
3. Klik op **toevoegen**.
4. In het dialoogvenster **Component toevoegen** :
    1. In de sectie **importeren** :
        1. Gebruik de knop **bestand** om te navigeren naar het certificaat dat u wilt gebruiken. 
        2. Voor **methode**, selecteert u **kopiëren**.
    2. **Als de naam**, klik op het tekstvak en accepteer de standaardnaam.
    3. In de sectie **Deploy** :
        1. Voor **methode**, selecteert u **kopiëren**.
        2. Alleen **naar map**, typt u **% JAVA_HOME %**.
    4. Het dialoogvenster **Toevoegen onderdeel** ziet er ongeveer als volgt uit.

        ![Certificaatonderdeel toevoegen][add_cert_component]

    5. Klik op **OK**.

Uw certificaat zou nu worden opgenomen in uw implementatie. Houd er rekening mee dat ongeacht of u het certificaat in het WAR-bestand insluiten of als een onderdeel toevoegen aan uw implementatie, moet u het certificaat uploaden naar uw naamruimte zoals is beschreven in de sectie [een certificaat tot de naamruimte ACS uploaden][] .

[What is ACS?]: #what-is
[Concepts]: #concepts
[Prerequisites]: #pre
[Create a Java web application]: #create-java-app
[Create an ACS Namespace]: #create-namespace
[Add Identity Providers]: #add-IP
[Add a Relying Party Application]: #add-RP
[Create Rules]: #create-rules
[Een certificaat uploaden naar uw ACS naamruimte]: #upload-certificate
[Review the Application Integration Page]: #review-app-int
[Configure Trust between ACS and Your ASP.NET Web Application]: #config-trust
[Add the ACS Filter library to your application]: #add_acs_filter_library
[Deploy to the compute emulator]: #deploy_compute_emulator
[Deploy to Azure]: #deploy_azure
[Next steps]: #next_steps
[Project-website]: http://wastarterkit4java.codeplex.com/releases/view/61026
[Het weergeven van SAML die het resultaat van de Azure-Service voor het beheer van Access]: /en-us/develop/java/how-to-guides/view-saml-returned-by-acs/
[Access Control-Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Windows Identity Foundation]: http://www.microsoft.com/download/en/details.aspx?id=17331
[Windows Identity Foundation SDK]: http://www.microsoft.com/download/en/details.aspx?id=4451
[Azure Management Portal]: https://manage.windowsazure.com
[acs_flow]: ./media/active-directory-java-authenticate-users-access-control-eclipse/ACSFlow.png

<!-- Eclipse-specific -->
[add_acs_filter_lib]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibrary.png
[add_acs_filter_lib_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryEmulator.png
[add_acs_filter_lib_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryProduction.png

[relying_party_realm_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmEmulator.png
[relying_party_return_url_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLEmulator.png
[relying_party_realm_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmProduction.png
[relying_party_return_url_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLProduction.png
[add_cert_component]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddCertificateComponent.png
[add_jsp_file_acs]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddJSPFileACS.png
[create_acs_hello_world]: ./media/active-directory-java-authenticate-users-access-control-eclipse/CreateACSHelloWorld.png
[add_token_signing_cert]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddTokenSigningCertificate.png
 
