<properties
   pageTitle="Lotus Domino-Connector | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe Microsofts Lotus Domino-Connector configureren."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="lotus-domino-connector-technical-reference"></a>Lotus Domino verbindingslijn technische verwijzing
In dit artikel worden de Lotus Domino verbindingslijn. Het artikel is van toepassing op de volgende producten:

- Microsoft identiteitsbeheer 2016 (MIM2016)
- Forefront identiteit Manager 2010 R2 (FIM2010R2)
    -   Moet hotfix 4.1.3671.0 of hoger [KB3092178](https://support.microsoft.com/kb/3092178)gebruiken.

De verbindingslijn is voor MIM2016 en FIM2010R2, een gedownload vanaf het [Microsoft Downloadcentrum](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-lotus-domino-connector"></a>Overzicht van de Lotus Domino-Connector
De verbindingslijn van Lotus Domino kunt u de synchronisatieservice integreren met van IBM Lotus Domino-server.

Hoog niveau vanuit het perspectief, van worden de volgende functies ondersteund door de huidige versie van de verbindingslijn:

Functie | Ondersteuning
--- | ---
Verbonden gegevensbron | Server: <li>Lotus Domino 8.5.x</li><li>Lotus Domino 9.x</li>Client:<li>Lotus Notes-9.x</li>
Scenario 's | <li>Beheer van productlevenscyclus object</li><li>Groepsbeheer</li><li>Wachtwoordbeheer</li>
Bewerkingen | <li>Volledige en Delta importeren</li><li>Exporteren</li><li>Stel en wachtwoord op HTTP-wachtwoord wijzigen</li>
Schema | <li>Persoon (Roaming gebruiker, contactpersoon (voor personen met geen certificaat))</li><li>Groep</li><li>Resource (kamer, Resource-onlinevergadering)</li><li>E-mail in database</li><li>Dynamische detectie van kenmerken voor ondersteunde objecten</li>

De verbindingslijn Lotus Domino wordt de Lotus Notes-client gebruikt om te communiceren met Lotus Domino-Server. Als gevolg van deze afhankelijkheid, moet een ondersteunde Lotus Notes-Client op de synchronisatieserver zijn geïnstalleerd. De communicatie tussen de client en de server wordt geïmplementeerd via de interface van Lotus Notes .NET Interop (Interop.domino.dll). Deze interface vergemakkelijkt de communicatie tussen de Microsoft.NET platform en Lotus Notes-client en ondersteuning biedt voor toegang tot Lotus Domino-documenten en weergaven. Voor delta importeren is het ook mogelijk dat de eigen interface C++ (afhankelijk van de geselecteerde delta importmethode) wordt gebruikt.

### <a name="prerequisites"></a>Vereisten voor
Voordat u de verbindingslijn gebruiken, te controleren of u de volgende handelingen uit op de synchronisatieserver:

- 4.5.2 van Microsoft .NET Framework of hoger
- De Lotus Notes-client moet zijn geïnstalleerd op de synchronisatieserver
- Het Lotus Domino-Connector nodig de standaard Lotus Domino LDAP schemadatabase (schema.nsf) op de Domino Directory-server zijn geïnstalleerd. Als niet aanwezig is, kunt u dit installeren door te voeren of opnieuw starten van de LDAP-service op de Domino-server.

### <a name="connected-data-source-permissions"></a>Verbonden gegevensbron machtigingen
Als u wilt een van de ondersteunde bewerkingen uitvoeren in Lotus Domino-connector, moet u lid zijn van de volgende groepen:

- Volledige toegang-beheerders
- Beheerders
- Databasebeheerders van de

De volgende tabel bevat de machtigingen die vereist voor elke bewerking zijn:

Bewerking | Toegangsrechten
--- | ---
Importeren | <li>Openbare documenten lezen</li><li> Access-beheerder volledig (wanneer u lid van de groep administrators volledige toegang bent, u automatisch de effectieve toegang hebt tot in ACL.)</li>
Exporteren en wachtwoord instellen | Effectieve Access: <li>Documenten maken</li><li>Documenten verwijderen</li><li>Openbare documenten lezen</li><li>Openbare documenten schrijven</li><li>Documenten repliceren of kopiëren</li>Voor het exporteren van gegevens moet u ook de volgende rollen: <li>CreateResource</li><li>GroupCreator</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li>

### <a name="direct-operations-and-adminp"></a>Directe bewerkingen en AdminP
Bewerkingen Ga rechtstreeks naar de map Domino of via het proces AdminP. De volgende tabellen objecten in de lijst alle ondersteund, bewerkingen en, indien van toepassing, de methode gerelateerde implementatie:

**Primaire-adresboek**

Object | Maken | Update | Verwijderen
--- | --- | --- | ---
Persoon | AdminP | Directe | AdminP
Groep | AdminP | Directe | AdminP
MailInDB | Directe | Directe | Directe
Resource | AdminP | Directe | AdminP

**Secundaire adresboek**

Object | Maken | Update | Verwijderen
--- | --- | --- | ---
Persoon | N/B | Directe | Directe
Groep | Directe | Directe | Directe
MailInDB | Directe | Directe | Directe
Resource | N/B | N/B | N/B

Wanneer een resource is gemaakt, wordt een document notities gemaakt. Op dezelfde manier wanneer een resource wordt verwijderd, wordt het document notities verwijderd.

### <a name="ports-and-protocols"></a>Poorten en protocollen
IBM Lotus Notes-client en Domino-servers communiceren met notities Remote Procedure Call (NRPC) waar NRPC TCP/IP moet gebruiken. Het standaardpoortnummer 1352 is, maar kan worden gewijzigd door de Domino-beheerder.

### <a name="not-supported"></a>Niet ondersteund
De volgende bewerkingen niet worden ondersteund door de huidige versie van de verbindingslijn Lotus Domino:

- Postvak verplaatsen tussen servers.

## <a name="create-a-new-connector"></a>Een nieuwe verbindingslijn maken

### <a name="client-software-installation-and-configuration"></a>Client-Software installeren en configureren
Lotus Notes moeten worden geïnstalleerd op de server **voordat u** die de verbindingslijn is geïnstalleerd.

Tijdens de installatie, moet dat u doen met een **Installatie voor één gebruiker**. De standaardinstelling **Installeren van meerdere gebruikers** werkt niet.  
![Notes1](./media/active-directory-aadconnectsync-connector-domino/notes1.png)

Op de pagina onderdelen installeert u alleen de vereiste Lotus Notes-functies en de **Client eenmalige aanmelding**. Eenmalige aanmelding is vereist voor de verbindingslijn moeten kunnen aanmelden bij de Domino-server.  
![Notes2](./media/active-directory-aadconnectsync-connector-domino/notes2.png)

**Notitie:** Lotus Notes eenmaal beginnen met een gebruiker die zich bevindt op dezelfde server als het account dat u als de service-account van de verbindingslijn gebruikt. Ook Zorg ervoor dat de Lotus Notes-client op de server. Dit kan niet worden uitgevoerd op hetzelfde moment die de verbindingslijn probeert verbinding te maken met de Domino-server.

### <a name="create-connector"></a>Verbindingslijn maken
Als u wilt maken van een verbindingslijn Lotus Domino, selecteert u in **Synchronisatieservice** **Management Agent** en **maken**. Selecteer de verbindingslijn **Lotus Domino (Microsoft)** .  
![CreateConnector](./media/active-directory-aadconnectsync-connector-domino/createconnector.png)

Als uw versie van de synchronisatieservice de mogelijkheid om te configureren **architectuur biedt**, controleert u of dat de verbindingslijn is ingesteld op de standaardwaarde uitvoeren in het **proces**.

### <a name="connectivity"></a>Connectiviteit
Klik op de pagina Connectivity moet u de naam van de Lotus Domino-server opgeven en de aanmeldingsreferenties invoeren.  
![Connectiviteit](./media/active-directory-aadconnectsync-connector-domino/connectivity.png)

De eigenschap Domino-Server ondersteunt twee typen opmaak voor de naam van de server:

- Servernaam
- Servernaam/DirectoryName

De **Servernaam/DirectoryName** -indeling is de gewenste notatie voor dit kenmerk omdat hiermee sneller antwoord wanneer de verbindingslijn contact op met de Domino-Server.

De meegeleverde gebruikers-id-bestand is opgeslagen in de configuratiedatabase van de synchronisatieservice.

Voor **Delta importeren** hebt u deze opties:

- **Geen**. De verbindingslijn doet geen delta invoer niet.
- **Toevoegen/bijwerken**. De verbindingslijn doet delta importeren kunt toevoegen en bijwerken van bewerkingen. Voor verwijderen is een **Volledige** importbewerking vereist. Deze bewerking wordt de .net interop gebruikt.
- **Toevoegen/bijwerken/verwijderen**. Het importeren van verbindingslijn doet delta toevoegen, bijwerken en verwijderen. Deze bewerking wordt de systeemeigen C++-interfaces gebruikt.

Opties voor **Schema** hebt u de volgende opties:

- **Schema standaard**. De verbindingslijn door het schema van de Domino-server gedetecteerd. Dit is de standaardoptie.
- **DSML-Schema**. Alleen gebruikt als de Domino-server door het schema niet worden. Vervolgens kunt u een bestand DSML maken met het schema en deze beter importeren. Zie voor meer informatie over DSML, [OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml).

Wanneer u op volgende hebt geklikt, wordt de gebruikersnaam en wachtwoord configuratieparameters worden gecontroleerd.

### <a name="global-parameters"></a>Globale Parameters
Klik op de pagina globale Parameters die u kunt configureren de gewenste tijdzone en het importeren en exporteren de optie bij bewerking.  
![Globale Parameters](./media/active-directory-aadconnectsync-connector-domino/globalparameters.png)

De parameter **tijdzone Domino-Server** Hiermee definieert u de locatie van de Domino-Server.

Deze configuratieoptie is vereist ter ondersteuning van bewerkingen **delta importeren** omdat de synchronisatie service wijzigingen tussen de laatste twee invoer bepalen.

#### <a name="import-settings-method"></a>Importinstellingen, methode
De **Volledige importeren uitvoeren door** heeft de volgende opties:

- Zoeken
- Weergave (aanbevolen)

**Zoeken** wordt gebruikt in de Domino indexeren maar dat is gebruikelijk dat de indexen niet worden bijgewerkt in realtime en de gegevens die zijn geretourneerd door de server niet altijd correct is. Voor een systeem met veel wijzigingen, deze optie meestal goed werkt niet en biedt ONWAAR worden verwijderd in sommige gevallen. **Tijdens het zoeken** is echter sneller dan **weergave**.

**Weergave** is de optie aanbevolen, omdat deze de juiste status van de gegevens bevat. Dit is iets langzamer dan zoeken.

#### <a name="creation-of-virtual-contact-objects"></a>Maken van virtuele contactpersonen objecten
De **maken van inschakelen \_contactpersoonobject** heeft de volgende opties:

- Geen
- Niet-naslag waarden
- Naslaggids voor niet waarden

In de Domino bevatten verwijzing kenmerken veel verschillende indelingen om te verwijzen naar andere objecten. Kunnen andere variaties, de verbindingslijn implementeert \_Neem contact op met objecten, ook bekend als **Virtuele contactpersonen** (VC). Deze objecten zijn gemaakt zodat ze kunnen deelnemen aan aan bestaande MV objecten of geprojecteerd als nieuwe objecten. Op deze manier kunnen kenmerk verwijzingen worden bewaard.

Doordat deze instelling en als de inhoud van een verwijzing-kenmerk niet een DN-notatie is, een \_contactpersoonobject wordt gemaakt. Een Lidkenmerk van een groep kan bijvoorbeeld SMTP-adressen bevatten. Het is ook mogelijk om korte naam en andere kenmerken die aanwezig zijn in de verwijzing kenmerken. Selecteer **Waarden voor niet-naslag**voor dit scenario. Deze configuratie is de meest voorkomende instelling voor Domino-implementaties.

Wanneer Lotus Domino is geconfigureerd om afzonderlijke adresboeken met verschillende DN-namen dat staat voor hetzelfde object, is het mogelijk ook maken \_contact opnemen met objecten voor alle verwijzing waarden die zijn gevonden in een adresboek. In dit scenario de optie **verwijzing en niet-naslag waarden** .

Als u meerdere waarden in de **volledige naam** in de Domino-kenmerk hebt, klikt u vervolgens gewenste ook voor het maken van virtuele contactpersonen zodat verwijzingen kunnen worden opgelost. Bijvoorbeeld, kan dit kenmerk meerdere waarden hebben na een huwelijk of echtscheiding. Schakel het selectievakje **inschakelen... Volledige naam heeft meerdere waarden** voor dit scenario.

Klik op de juiste kenmerken, het samenvoegen van de \_contactpersoonobjecten zou lid zijn van het object MV.

Deze objecten hebben VC =\_contactpersoon wordt toegevoegd aan hun DN-naam.

#### <a name="import-settings-conflict-object"></a>Importinstellingen, conflicteren object
**Uitsluiten Conflict-Object**

In een grote Domino-implementatie is het mogelijk dat meerdere objecten de dezelfde DN vanwege replicatie op te lossen hebben. In dat geval wilt de verbindingslijn zien van twee objecten met verschillende UniversalIDs, maar dezelfde DN-naam. Dit conflict zou leiden tot een tijdelijke object wordt gemaakt in de ruimte verbindingslijn. De verbindingslijn kunt de objecten die zijn geselecteerd in de Domino als herhaling slachtoffer negeren. De aanbeveling is dat dit selectievakje is geselecteerd.

#### <a name="export-settings"></a>Instellingen van exporteren
Als de optie **AdminP gebruiken voor het bijwerken van verwijzingen** niet ingeschakeld is, exporteren uit verwijzing kenmerken, zoals lid is van een directe oproep en geen gebruikmaakt van het proces AdminP. Gebruik deze optie alleen wanneer AdminP niet is geconfigureerd voor het behoud van referentiële integriteit.

#### <a name="routing-information"></a>Informatie over de routering
In de Domino is het mogelijk dat een kenmerk verwijzing informatie over de routering ingesloten als achtervoegsel aan de DN heeft. Bijvoorbeeld het Lidkenmerk in een groep kan bevatten **CN=example/organization@ABC**. Het achtervoegsel @ABC vindt u informatie over de routering. De informatie over de routering wordt gebruikt door Domino e-mailberichten verzenden naar de juiste Domino-systeem, die een systeem in een andere organisatie kan zijn. In het veld informatie routering kunt u de routering achtervoegsels binnen de organisatie in het bereik van de verbindingslijn gebruikt. Als een van deze waarden in een verwijzing-kenmerk als achtervoegsel wordt gevonden, wordt de informatie over de routering verwijderd uit de verwijzing. Als het routeren achtervoegsel op de waarde van een verwijzing niet met een van deze waarden die zijn opgegeven overeen, een \_contactpersoonobject wordt gemaakt. Deze \_contactpersoonobjecten zijn gemaakt met ** RO=@ ** ingevoegd in de DN. Voor deze \_contactpersoon objecten de volgende kenmerken worden ook toegevoegd aan het deelnemen aan een reële object eventueel toestaan: \_routingName, \_contactpersoon, \_weergavenaam, en UniversalID.

#### <a name="additional-address-books"></a>Extra adresboeken
Als u nog geen **ondersteuning voor directory** hebt geïnstalleerd, waarmee de naam van secundaire adresboeken, kunt u deze adresboeken handmatig invoeren.

#### <a name="multivalued-transformation"></a>Met meerdere waarden transformatie
Veel kenmerken in Lotus Domino zijn met meerdere waarden. De bijbehorende metaverse-kenmerken worden meestal één waarden. Door het importeren en de optie van de bewerking exporteren te configureren, moet u de verbindingslijn om u te helpen met de vereiste vertaling van de desbetreffende kenmerken inschakelen.

**Exporteren**  
De optie exporteren-bewerking ondersteunt twee modi:

- Item toevoegen
- Item vervangen

**Item vervangen** – wanneer u deze optie selecteert, de verbindingslijn altijd verwijderen van de huidige waarden van het kenmerk in Domino en vervang deze velden door de opgegeven waarden. De opgegeven waarde kunt werken met één waarde of met meerdere waarden.

Voorbeeld: Het kenmerk assistent van een persoon-object heeft de volgende waarden:

- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Als u een nieuwe assistent **David Alexander** met de naam is toegewezen aan deze persoon-object, wordt het resultaat is:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf

**Item toevoegen** – wanneer u deze optie selecteert, de verbindingslijn behoudt de bestaande waarden in het kenmerk in Domino en voeg nieuwe waarden boven aan de lijst met gegevens.

Voorbeeld: Het kenmerk assistent van een persoon-object heeft de volgende waarden:

- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Als u een nieuwe assistent **David Alexander** met de naam is toegewezen aan deze persoon-object, wordt het resultaat is:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

**Importeren**  
De optie importeren bewerking ondersteunt twee modi:

- Standaard
- Met meerdere waarden enkele waarde

Alle waarden met alle kenmerken **standaard** – wanneer u de standaardoptie selecteert, worden geïmporteerd.

**Multivalued enkele waarde** – wanneer u deze optie, een kenmerk met meerdere waarden selecteert is geconverteerd naar een kenmerk met één waarde. Als er meer dan één waarde voorkomt, wordt de waarde in de rechterbovenhoek (deze waarde is meestal ook de meest recente) gebruikt.

Voorbeeld: Het kenmerk assistent van een persoon-object heeft de volgende waarden:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

De meest recente update aan dit kenmerk is **David Alexander**. Omdat de optie importeren bewerking is ingesteld op Multivalued enkele waarde, importeert verbindingslijn **David Alexander** alleen in de ruimte verbindingslijn.

De logica en met meerdere waarden kenmerken omzetten in één waarde kenmerken geldt niet voor het kenmerk group lid en voor het kenmerk van de volledige naam persoon.

Het ook mogelijk om te configureren importeren en exporteren van regels transformatie voor kenmerken per kenmerk met meerdere waarden, als een uitzondering op de globale regel. Als u wilt deze optie configureren, voer [objecttype]. [kenmerknaam] in de **lijst met uitsluitingen kenmerk importeren** en **exporteren van lijst met uitsluitingen kenmerk** tekstvakken. Als u Person.Assistant invoeren en de globale vlag is ingesteld op het importeren van alle waarden, wordt bijvoorbeeld alleen de eerste waarde voor de assistent geïmporteerd.

#### <a name="certifiers"></a>Certifiers
Alle organisatie/bedrijf eenheden worden doorvoeren en snel aanvullen vermeld. Als u wilt kunnen persoon objecten exporteren naar het primaire-adresboek, is een certifier met het wachtwoord vereist.

Als alle certifiers hetzelfde wachtwoord, kan het **wachtwoord voor alle Certifers** kan worden gebruikt. Vervolgens kunt u het wachtwoord hier invoeren en alleen Geef het bestand certifier.

Als u alleen importeert, klik hoeft u niet om op te geven van een certifiers.

### <a name="configure-provisioning-hierarchy"></a>Inrichten hiërarchie configureren
Als u de verbindingslijn Lotus Domino configureert, gaat u verder in dit dialoogvenster weer. De verbindingslijn Lotus Domino biedt geen ondersteuning voor hiërarchie inrichten.  
![Hiërarchie inrichting](./media/active-directory-aadconnectsync-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>Partities en hiërarchieën configureren
Als u partities en hiërarchieën configureert, moet u het primaire adresboek NAB=names.nsf genoemd. Naast het primaire adresboek, kunt u secundaire adresboeken indien aanwezig.  
![Partities](./media/active-directory-aadconnectsync-connector-domino/partitions.png)

### <a name="select-attributes"></a>Selecteer kenmerken
Als u uw kenmerken configureert, moet u alle kenmerken die worden voorafgegaan door ** \_MMS\_**. Deze kenmerken zijn vereist wanneer u nieuwe objecten op Lotus Domino inrichten

![Kenmerken](./media/active-directory-aadconnectsync-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>Beheer van productlevenscyclus object
Deze sectie bevat een overzicht van de andere objecten in de Domino.

### <a name="person-objects"></a>Persoon objecten
Het object persoon vertegenwoordigt gebruikers in de organisatie en organisatie-eenheden. Naast de standaardattributen kunt de Domino-beheerder aangepaste kenmerken toevoegen aan een object persoon. Minimaal moet een persoon object alle verplichte kenmerken opnemen. Zie voor een volledige lijst van verplichte kenmerken, [Lotus Notes-eigenschappen](#lotus-notes-properties). Als u wilt een object persoon hebt geregistreerd, is de volgende vereisten voldaan:

- Het adresboek (names.nsf) moet zijn gedefinieerd en moet de primaire-adresboek.
- U moet de O/OU certifier Id en het wachtwoord voor het registreren van een bepaalde gebruiker in de organisatie / organisatie-eenheid.
- U moet een specifieke reeks Lotus Notes-eigenschappen voor een persoon-object instellen. Deze eigenschappen worden gebruikt voor het inrichten van het object persoon. Zie de sectie met de naam van [Lotus Notes-eigenschappen](#lotus-notes-properties) verderop in dit document voor meer informatie.
- De eerste HTTP-wachtwoord voor een persoon is een kenmerk en de instellen tijdens het inrichten.
- Het object persoon moet een van de volgende drie ondersteunde typen:
    1. Normaal-gebruiker met een e-bestand en een gebruikers-id-bestand
    2. Roaming gebruiker (een normaal-gebruiker met alle roaming databasebestanden)
    3. Contactpersonen (gebruiker met geen id-bestand)

Personen (behalve contactpersonen) kunnen verder worden gegroepeerd in ons gebruikers en internationale gebruikers, zoals gedefinieerd door de waarde van de \_MMS\_IDRegType eigenschap. Deze personen gebruiken de Notes-Client voor toegang tot Lotus Domino-servers hebben een notities-Id en het document van een persoon. Als ze notities e-mail gebruikt, klikt u vervolgens hebt worden ook u een e-bestand. De gebruiker moet zijn geregistreerd voor actief. Zie voor meer informatie:

- [Notes-gebruikers instellen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
- [Gebruiker registreren](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
- [Gebruikers beheren](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
- [De naam van gebruikers wijzigen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_USER_AUTOMATICALLY.html)

Alle deze bewerkingen worden uitgevoerd in Lotus Domino en klik vervolgens in de synchronisatieservice zijn geïmporteerd.

### <a name="resources-and-rooms"></a>Resources en ruimten
Een Resource is een ander type van een database in Lotus Domino. Resources zijn vergaderruimten bij verschillende gegevenstypen van apparatuur, zoals projectors. Er zijn subtypen van resources die worden ondersteund door Lotus Domino-connector die zijn gedefinieerd door het kenmerk Type Resource:

Type Resource | Kenmerk Type resource
--- | ---
Ruimte | 1
Resource (overige) | 2
Onlinevergadering | 3

De volgende is vereist voor het type Resource object om te werken:

- Resource reserveringsdatabase moet al bestaan in de verbonden Domino-server
- De site is al gedefinieerd voor de Resource

De database Resource reserveren bevat drie soorten documenten:

- Het profiel van site
- Resource
- Het reserveren

Zie voor meer informatie over het instellen van Resource reserveren database, [bij het instellen van de Resource apparatuurreserveringen-database](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html).

**Maken, bijwerken en verwijderen van Resources**  
De bewerkingen maken, bijwerken en verwijderen zijn uitgevoerd door de verbindingslijn Lotus Domino in de database Resource reserveren. Resources worden gemaakt als documenten in Names.nsf (dat wil zeggen de primaire adresboek). Zie voor meer informatie over het bewerken en verwijderen van Resources, [bewerken en verwijderen van documenten van de Resource](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html).

**Importeren en exporteren van de bewerking voor Resources**  
De bronnen kunnen worden geïmporteerd naar en geëxporteerd vanaf de synchronisatieservice, zoals een ander objecttype. Selecteer het objecttype als Resource tijdens de configuratie. Op de exportbewerking is geslaagd, moet u de details van het type Resource, telefonische Database en de naam van de Site hebben.

### <a name="mail-in-databases"></a>E-mail In Databases
Een Database E-mail In is een database die is bedoeld voor het ontvangen van e-mailberichten. Het is een Lotus Domino-postvak dat is niet gekoppeld aan een specifieke Lotus Domino-gebruikersaccount (dat wil zeggen het heeft geen eigen bestand ID en wachtwoord). Een database e-mail in heeft een unieke gebruikers-id ("korte naam") die zijn gekoppeld aan dit en een eigen e-mailadres.

Als er is een nodig voor een afzonderlijk Postvak met een eigen e-mailadres tussen verschillende gebruikers delen kunt (bijvoorbeeld group@contoso.com), een e-mail in-database is gemaakt. De toegang tot dit postvak wordt bepaald door de Access (Toegangsbeheerlijst), waarin de namen van de notities-gebruikers die zijn toegestaan het postvak te openen.

Zie de sectie [Verplichte kenmerken](#mandatory-attributes) verderop in dit artikel voor een lijst met de vereiste kenmerken.

Wanneer u een database is ontworpen om een e-mail te ontvangen, wordt een document E-mail In de Database gemaakt in Lotus Domino. In dit document moet bestaan in de map Domino van elke server waarop een kopie van de database is opgeslagen. Zie [een document E-mail In de Database te maken](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html)voor een gedetailleerde beschrijving over het maken van een databasedocument e-mail in de.

Voordat u een Database E-mail In maakt, moet de database al bestaan (moeten worden gemaakt door de beheerder van de Lotus) op de Domino-server.

### <a name="group-management"></a>Groepsbeheer
U kunt een gedetailleerd overzicht van het beheer van de groep Lotus Domino krijgen van de volgende bronnen:

- [Gebruik van groepen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
- [Een groep maken](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
- [Maken en groepen wijzigen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
- [Groepen beheren](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
- [De naam van een groep wijzigen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>Wachtwoordbeheer
Er zijn twee soorten wachtwoorden voor een geregistreerde Lotus Domino-gebruiker:

1. Gebruikerswachtwoord (opgeslagen in User.id-bestand)
2. Internet / HTTP-wachtwoord

De verbindingslijn Lotus Domino ondersteunt alleen bewerkingen met HTTP-wachtwoord.

Als u wilt uitvoeren wachtwoordbeheer, moet u wachtwoordbeheer voor de verbindingslijn in de ontwerpfunctie voor Management Agent inschakelen. Als u wachtwoordbeheer, selecteert u **Wachtwoordbeheer inschakelen** op de pagina **Configure Extensions** -dialoogvenster.  
![Uitbreidingen configureren](./media/active-directory-aadconnectsync-connector-domino/configureextensions.png)

De verbindingslijn Lotus Domino ondersteuning bewerkingen op internetwachtwoord te volgen:

- Wachtwoord instellen: Set-wachtwoord Hiermee stelt u een nieuw wachtwoord voor HTTP/Internet op de gebruiker in de Domino. Het account is al dan niet standaard ook ontgrendeld. De vlag ontgrendelen worden weergegeven op de WMI-interface van de synchronisatie-Engine.
- Wachtwoord wijzigen: In dit scenario een gebruiker eventueel het wachtwoord wijzigen of wachtwoord wijzigen na een opgegeven tijd wordt gevraagd. Voor deze bewerking uitvoeren plaatsen, zowel (de oude en het nieuwe wachtwoord) verplicht zijn. Nadat u gewijzigd, wordt het nieuwe wachtwoord in Lotus Domino bijgewerkt.

Zie voor meer informatie:

- [Gebruik van de functie voor Internet-accountvergrendeling](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
- [Internetwachtwoorden beheren](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>Naslaginformatie
In deze sectie bevat zoals beschrijvingen van kenmerken en kenmerk vereisten voor de verbindingslijn Lotus Domino.

### <a name="lotus-notes-properties"></a>Lotus Notes-eigenschappen
Wanneer u uw adreslijst Lotus Domino persoon objecten inrichten, moeten uw objecten een bepaalde set met eigenschappen hebben met specifieke waarden ingevuld. Deze waarden zijn alleen vereist voor bewerkingen maakt.

De volgende tabel worden deze eigenschappen en wordt een beschrijving gegeven van deze.

Eigenschap | Beschrijving
--- | ---
\_MMS_AltFullName | De alternatieve volledige naam van de gebruiker.
\_MMS_AltFullNameLanguage | De taal die u wilt worden gebruikt voor het opgeven van de alternatieve volledige naam van de gebruiker.
\_MMS_CertDaysToExpire | Het aantal dagen vanaf de huidige datum vóór het certificaat verloopt. Als u niet opgeeft, is de standaardnotatie voor datum twee jaar vanaf de huidige datum.
\_MMS_Certifier | De eigenschap die de naam van de organisatie-hiërarchie van de certifier bevat. Bijvoorbeeld: OU = OrganizationUnit, O = organigram, C = land.
\_MMS_IDPath | Als de eigenschap leeg is, wordt geen gebruiker identificatiebestand lokaal gemaakt op de Server synchroniseren. Als de eigenschap een bestandsnaam bevat, wordt een gebruikers-ID-bestand in de map madata gemaakt. De eigenschap kan ook een volledig pad bevatten.
\_MMS_IDRegType | Personen kunnen worden geclassificeerd als contactpersonen, ons gebruikers en internationale gebruikers. De volgende tabel bevat de mogelijke waarden: <li>0 - contactpersoon</li><li>1 - nl-gebruiker</li><li>2 - internationale gebruiker</li>
\_MMS_IDStoreType | De eigenschap is vereist voor de Verenigde Staten en internationale gebruikers. De eigenschap bevat een geheel getal dat of de gebruikers-id wordt opgeslagen als een bijlage in het adresboek notities of in de mail-bestand van de persoon. Als de gebruikers-ID-bestand een bijlage in het adresboek is, deze (optioneel) kan worden gemaakt als een bestand met \_MMS_IDPath. <li>Lege - Store-ID-bestand in ID kluis, geen identificatiebestand (gebruikt voor contactpersonen).</li><li> 1 - bijlage in het adresboek van notities. De \_MMS_Password eigenschap moet worden ingesteld voor gebruiker identificatie bestanden die bijlagen zijn</li><li>2 - ID in iemands mailbestand opslaan. De \_MMS_UseAdminP moet zijn ingesteld op onwaar om te laten van het e-bestand worden gemaakt tijdens de registratie van de persoon. De \_MMS_Password eigenschap moet worden ingesteld voor gebruikers-id-bestanden.</li>
\_MMS_MailQuotaSizeLimit | Het aantal megabytes op dat is toegestaan voor de e-bestand-database.
\_MMS_MailQuotaWarningThreshold | Het aantal megabytes op dat is toegestaan voor de e-bestand database voordat wordt een waarschuwing.
\_MMS_MailTemplateName | Het e-sjabloonbestand die wordt gebruikt voor het maken van de gebruiker e-bestand. Als een sjabloon is opgegeven, wordt de mailbestand gemaakt met de opgegeven sjabloon. Als er geen sjabloon is opgegeven, wordt het standaardsjabloonbestand wordt gebruikt om het bestand te maken.
\_MMS_OU | Optionele eigenschap met de naam van de organisatie-eenheid onder de certifier. Deze eigenschap moet leeg voor contactpersonen.
\_MMS_Password | De eigenschap is vereist voor gebruikers. De eigenschap bevat het wachtwoord voor het identificatiebestand van het object.
\_MMS_UseAdminP | Eigenschap moet worden ingesteld op waar als het e-bestand moet worden gemaakt door het proces AdminP op de Domino-server (asynchroon naar het exportproces). Als de eigenschap is ingesteld op ONWAAR, wordt het e-bestand gemaakt met de Domino-gebruiker (synchroon tijdens het exportproces).

Voor een gebruiker met een gekoppeld identificatiebestand, de \_MMS_Password eigenschap moet een waarde bevatten. Voor toegang tot e-mail via de Lotus Notes-client, moeten de e-MailServer en MailFile eigenschappen van een gebruiker een waarde bevatten.

Voor toegang tot e-mail via een webbrowser, moeten de volgende eigenschappen waarden bevatten:

- MailFile - eigenschap vereist waarin het pad op de Lotus Domino-server waar de e-mail-bestand is opgeslagen.
- E-MailServer - eigenschap vereist die de naam van de Lotus Domino-server bevat. Deze waarde is de naam moet worden gebruikt wanneer u het bestand Lotus mail op de Domino-server hebt gemaakt.
- HTTPPassword - optionele eigenschap die het wachtwoord van het access Web voor het object bevat.

Voor toegang tot de Domino-Server zonder mail-toepassing, moet de eigenschap HTTPPassword een waarde bevatten. De eigenschappen MailFile en het e-MailServer kunnen niet leeg zijn.

Met \_MMS_ IDStoreType = 2 (store id in E-mail-bestand), de eigenschap MailSystem van NotesRegistrationclass is ingesteld op REG_MAILSYSTEM_INOTES (3).

### <a name="mandatory-attributes"></a>Verplichte kenmerken
De verbindingslijn Lotus Domino ondersteunt voornamelijk deze typen objecten (documenttypen):

- Groep
- E-mail In Database
- Persoon
- Contactpersoon (iemand met geen certifier)
- Resource

In deze sectie bevat de kenmerken die verplicht voor elk ondersteunde object zijn exporteren naar een Domino-server.

Objecttype | Verplichte kenmerken
--- | ---
Groep | <li>ListName</li>
Database-hoofdmenu-In | <li>Volledige naam</li><li>MailFile</li><li>E-MailServer</li><li>MailDomain</li>
Persoon | <li>Achternaam</li><li>MailFile</li><li>Korte naam</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li>
Contactpersoon (iemand met geen certifier) | <li>\_MMS_IDRegType</li>
Resource | <li>Volledige naam</li><li>Brontype</li><li>ConfDB</li><li>Resourcecapaciteit</li><li>Site</li><li>Weergavenaam</li><li>MailFile</li><li>E-MailServer</li><li>MailDomain</li>

## <a name="common-issues-and-questions"></a>Veelvoorkomende problemen en vragen

### <a name="schema-detection-does-not-work"></a>Schema detectie werkt niet
Als u wilt kunnen vinden het schema, moet dat het bestand schema.nsf op de server Domino aanwezig is. Dit bestand wordt alleen weergegeven als LDAP op de server is geïnstalleerd. Als het schema detecteerbare is, controleert u het volgende:

- Het bestand schema.nsf is aanwezig op de hoofdmap van de Domino-Server
- De gebruiker heeft machtigingen om het bestand schema.nsf weer te geven.
- Ervoor zorgen dat een herstart van de LDAP-server. Open **Lotus Domino-Console** en gebruik **Vertel LDAP ReloadSchema** -opdracht om het schema opnieuw laden.

### <a name="not-all-secondary-address-books-are-visible"></a>Niet alle secundaire adresboeken zijn zichtbaar
De verbindingslijn Domino, is afhankelijk van de functie **Telefoonboek** moeten kunnen de secundaire adresboeken zoeken. Als de secundaire adresboeken ontbreken, controleert u of als [Telefoonboek](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_DIRECTORY_ASSISTANCE.html) is ingeschakeld en geconfigureerd op de Domino-Server.

### <a name="custom-attributes-in-domino"></a>Aangepaste kenmerken in Domino
Er zijn verschillende manieren in Domino voor het uitbreiden van het schema, zodat deze wordt weergegeven als een aangepaste kenmerk verbruikbare doorvoeren en snel aanvullen.

**Methode 1: Lotus Domino schema uitbreiden**

1. Een kopie maken van Domino Directory sjabloon {PUBNAMES. NTF} door de volgende [deze stappen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (u moet niet aanpassen de standaardmap voor IBM Lotus Domino sjabloon):
2. Open de kopie van Domino directory sjabloon {CONTOSO. De sjabloon NTF} die is gemaakt in de Domino Designer en als volgt te werk:
    - Klik op gedeeld elementen en vouwen van subformulieren
    - Dubbelklik op {objectnaam} $InheritableSchema subformulier (waar is {Objectnaam} de naam van de standaard structurele objectklasse, bijvoorbeeld: persoon).
    - De naam van het kenmerk dat u wilt toevoegen in het schema {MyPersonAtrribute} en de bijbehorende aan dat kenmerk. Een veld maken met Selecteer het Menu **maken** en selecteer vervolgens **veld** in het menu.
    - In het veld toegevoegde eigenschappen ervan te instellen door te selecteren van het Type, de stijl, grootte, lettertype en andere gerelateerde parameters op eigenschappenvenster veld.
    - Het kenmerk standaardwaarde is dezelfde als de naam die is opgegeven voor dat kenmerk behouden (bijvoorbeeld als de kenmerknaam van het MyPersonAttribute is, behoud de standaardwaarde met dezelfde naam).
    - Sla het ${objectnaam} InheritableSchema subformulier met de bijgewerkte waarden.
3. Vervang de Domino Directory sjabloon {PUBNAMES. NTF} met de nieuwe aangepaste sjabloon {CONTOSO. NTF} door de volgende [deze stappen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
4. Sluit Domino beheerder en Domino-Console om de LDAP-service opnieuw te starten en de LDAP-Schema opnieuw laden hebt geopend:
    - Invoegen in de Domino-Console, de opdracht onder de tekst van de **Opdracht Domino** is opgeslagen om de LDAP-service - [Taak LDAP opnieuw]( http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html)opnieuw te starten.
    - Om opnieuw te laden LDAP schema Gebruik LDAP Vertel opdracht - LDAP-ReloadSchema vertellen
5. Open Domino-beheerder en selecteer het tabblad personen en groepen te zien toegevoegde kenmerk is doorgevoerd in de domino persoon toevoegen.
6. Open Schema.nsf van tabblad **bestanden** en Zie toegevoegde kenmerk is doorgevoerd in klasse dominoPerson LDAP-object.

**Methode 2: Een auxClass met aangepaste kenmerk maken en koppelen aan de objectklasse**

1. Een kopie maken van Domino Directory sjabloon {PUBNAMES. NTF} door [deze stappen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) van de volgende (nooit aanpassen de standaardmap voor IBM Lotus Domino sjabloon):
2. Open de kopie van Domino directory sjabloon {CONTOSO. De sjabloon NTF} die is gemaakt, klikt u in de Domino Designer.
3. Selecteer in het linkerdeelvenster Code gedeeld en klik vervolgens op subformulieren.
4. Klik op nieuw subformulier
5. Het volgende als u wilt opgeven van de eigenschappen voor het nieuwe subformulier doen:
    - Met het nieuwe subformulier geopend, kiest u ontwerp - eigenschappen van een subformulier
    - Naast de naam van eigenschap, voer een naam voor de klas Aux object--bijvoorbeeld TestSubform.
    - De eigenschap Options behouden "Opnemen in invoegen subformulier... dialoogvenster" geselecteerd
    - Schakel de opties eigenschap "Render pass through-HTML in notities."
    - De andere eigenschappen ongewijzigd wilt laten en sluit het dialoogvenster subformulier.
    - Opslaan en sluiten van het nieuwe subformulier.
6. Voer de volgende handelingen uit als u wilt toevoegen van een veld voor de klas Aux object definiëren:
    - Open het subformulier die u hebt gemaakt.
    - Kies maken - veld.
    - Klik naast de naam op het tabblad basis van het dialoogvenster veld Geef een naam, bijvoorbeeld: {MyPersonTestAttribute}.
    - Stel de eigenschappen door te selecteren de Type, de stijl, de grootte, het lettertype en de gerelateerde eigenschappen in het veld is toegevoegd.
    - Het kenmerk standaardwaarde is dezelfde als de naam die is opgegeven voor dat kenmerk behouden (bijvoorbeeld als de kenmerknaam van het MyPersonTestAttribute is, behoud de standaardwaarde met dezelfde naam).
    - Het subformulier opslaan met de bijgewerkte waarden en voer de volgende handelingen uit:
        - Selecteer in het linkerdeelvenster Code gedeeld en klik vervolgens op subformulieren
        - Selecteer het nieuwe subformulier en kies ontwerp - ontwerpeigenschappen.
        - Klik op het derde tabblad aan de linkerkant en selecteer **doorgeven deze verboden ontwerp wijzigen**.
7. Open ${objectnaam} ExtensibleSchema subformulier, ({objectnaam} is waar de naam van de standaard structurele objectklasse, bijvoorbeeld – persoon).
8. Resource invoegen, selecteer het subformulier (die u hebt gemaakt, bijvoorbeeld – TestSubform) en het subformulier van ${objectnaam} ExtensibleSchema opslaan.
9. Vervang de Domino Directory sjabloon {PUBNAMES. NTF} met de nieuwe aangepaste sjabloon {CONTOSO. NTF} door de volgende [deze stappen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
10. Sluit Domino beheerder en Domino-Console om de LDAP-service opnieuw te starten en de LDAP-Schema opnieuw laden hebt geopend:
    - Invoegen in de Domino-Console, de opdracht onder de tekst van de **Opdracht Domino** is opgeslagen om de LDAP-service - [Taak LDAP opnieuw](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html)opnieuw te starten.
    - Om opnieuw te laden LDAP gebruik schema Vertel LDAP-opdracht **LDAP-ReloadSchema zien**.
11. Open Domino-beheerder en selecteer personen en groepen tabblad om extra kenmerk is doorgevoerd in de domino toevoegen persoon weer te geven (onder anderen tab).
12. Open Schema.nsf van tabblad **bestanden** en Zie toegevoegde kenmerk wordt aangegeven bij TestSubform LDAP Aux object class.

**Methode 3: Het aangepaste kenmerk toevoegen aan de klasse ExtensibleObject**

1. Bestand openen {Schema.nsf} in de hoofdmap is geplaatst
2. Selecteer LDAP objectklassen in het linkermenu onder **Alle Schema documenten** en klikt u op de knop **klasse Object toevoegen** :
3. Geef de LDAP-naam in de vorm van {zzzExtensibleSchema} (zzz is waar de naam van de standaard structurele objectklasse, bijvoorbeeld persoon). Bijvoorbeeld schema uitbreiden voor persoon objectklasse, bieden u LDAP-naam {PersonExtensibleSchema}.
4. Bovenliggend Object klassenaam, waarvoor u het schema uitbreidt wilt bieden. Schema voor persoon object klas uitbreiden, bieden u bijvoorbeeld bovenliggend class objectnaam {dominoPerson}:
5. Geef een geldige OID overeenkomt met de objectklasse.
6. Selecteer uitgebreid/aangepaste kenmerken onder verplicht of optioneel kenmerktypen velden aan de hand van de vereiste:
7. Na het toevoegen van vereiste kenmerken naar de ExtensibleObjectClass, klikt u op **Opslaan en sluiten**.
8. Een ExtensibleObjectClass is gemaakt voor de desbetreffende standaard objectklasse met uitgebreide kenmerken.

## <a name="troubleshooting"></a>Problemen oplossen

-   Zie voor informatie over het inschakelen van logboekregistratie om op te lossen de verbindingslijn, [hoe u ETW tracering inschakelen voor verbindingslijnen](http://go.microsoft.com/fwlink/?LinkId=335731).
