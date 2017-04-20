<properties
   pageTitle="Algemene LDAP-Connector | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe Microsofts algemene LDAP-Connector configureren."
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

# <a name="generic-ldap-connector-technical-reference"></a>Algemene LDAP-Connector technische verwijzing
In dit artikel worden de algemene LDAP-verbindingslijn. Het artikel is van toepassing op de volgende producten:

- Microsoft identiteitsbeheer 2016 (MIM2016)
- Forefront identiteit Manager 2010 R2 (FIM2010R2)
    -   Moet hotfix 4.1.3671.0 of hoger [KB3092178](https://support.microsoft.com/kb/3092178)gebruiken.

De verbindingslijn is voor MIM2016 en FIM2010R2, een downloaden via het [Microsoft Downloadcentrum](http://go.microsoft.com/fwlink/?LinkId=717495).

Wanneer wordt verwezen naar IETF RFC, dit document is worden gebruikt voor het gebruik van de indeling (RFC [RFC aantal] / [sectie in RFC document]), bijvoorbeeld (RFC 4512/4.3).
U kunt meer informatie vinden op http://tools.ietf.org/html/rfc4500 (u moet vervangen door het juiste RFC-nummer 4500).

## <a name="overview-of-the-generic-ldap-connector"></a>Overzicht van de algemene LDAP-Connector
De algemene LDAP-Connector kunt u de synchronisatieservice integreren met een LDAP-server v3.

Bepaalde acties en schema-elementen, zoals die nodig zijn om uit te voeren delta importeren, zijn niet opgegeven in de IETF RFC's. Voor deze bewerkingen, worden alleen LDAP-adreslijsten expliciet is opgegeven ondersteund.

Hoog niveau vanuit het perspectief, van worden de volgende functies ondersteund door de huidige versie van de verbindingslijn:

Functie | Ondersteuning
--- | --- |
Verbonden gegevensbron | De verbindingslijn wordt ondersteund met alle LDAP v3-servers (RFC 4510 compatibele). Het is getest met de volgende items: <li>Microsoft Active Directory Lightweight Directory Services (AD LDS)</li><li>Microsoft Active Directory wereldwijde catalogus (AD ° c)</li><li>389 directory-Server</li><li>Apache Directory-Server</li><li>IBM Tivoli DS</li><li>Isode Directory</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>Open DJ</li><li>Open DS</li><li>Open LDAP (openldap.org)</li><li>Oracle (eerder zondag) Directory Server Enterprise Edition</li><li>RadiantOne virtuele map Server (VDS)</li><li>Zo een Directory-Server</li>**Aantal aanzienlijke mappen worden niet ondersteund:** <li>Microsoft Active Directory Domain Services (AD DS) [gebruik in plaats hiervan de ingebouwde Active Directory-Connector]</li><li>Oracle-internetadreslijst (OID)</li>
Scenario 's   | <li>Beheer van productlevenscyclus object</li><li>Groepsbeheer</li><li>Wachtwoordbeheer</li>
Bewerkingen |De volgende bewerkingen worden ondersteund in alle LDAP-mappen: <li>Volledige importeren</li><li>Exporteren</li>De volgende bewerkingen worden alleen ondersteund op opgegeven mappen:<li>Delta importeren</li><li>Wachtwoord instellen, wachtwoord wijzigen</li>
Schema | <li>Schema wordt aangetroffen uit het schema LDAP (RFC3673 en RFC4512/4.2)</li><li>Ondersteunt structurele klassen, aux klassen en extensibleObject object class (RFC4512/4.3)</li>

### <a name="delta-import-and-password-management-support"></a>Delta ondersteuning voor importeren en wachtwoord
Mappen voor Delta importeren en wachtwoordbeheer ondersteund:

- Microsoft Active Directory Lightweight Directory Services (AD LDS)
    - Ondersteuning biedt voor alle bewerkingen voor delta importeren
    - Ondersteunt het wachtwoord instellen
- Microsoft Active Directory wereldwijde catalogus (AD ° c)
    - Ondersteuning biedt voor alle bewerkingen voor delta importeren
    - Ondersteunt het wachtwoord instellen
- 389 directory-Server
    - Ondersteuning biedt voor alle bewerkingen voor delta importeren
    - Ondersteunt het wachtwoord en wachtwoord wijzigen instellen
- Apache Directory-Server
    - Biedt geen ondersteuning voor delta importeren omdat deze map geen een wijzigingenlogboek permanente heeft
    - Ondersteunt het wachtwoord instellen
- IBM Tivoli DS
    - Ondersteuning biedt voor alle bewerkingen voor delta importeren
    - Ondersteunt het wachtwoord en wachtwoord wijzigen instellen
- Isode Directory
    - Ondersteuning biedt voor alle bewerkingen voor delta importeren
    - Ondersteunt het wachtwoord en wachtwoord wijzigen instellen
- Novell eDirectory en NetIQ eDirectory
    - Ondersteunt het toevoegen, bijwerken en naam bewerkingen voor delta importeren
    - Biedt geen ondersteuning voor Delete-bewerkingen voor delta importeren
    - Ondersteunt het wachtwoord en wachtwoord wijzigen instellen
- Open DJ
    - Ondersteuning biedt voor alle bewerkingen voor delta importeren
    - Ondersteunt het wachtwoord en wachtwoord wijzigen instellen
- Open DS
    - Ondersteuning biedt voor alle bewerkingen voor delta importeren
    - Ondersteunt het wachtwoord en wachtwoord wijzigen instellen
- Open LDAP (openldap.org)
    - Ondersteuning biedt voor alle bewerkingen voor delta importeren
    - Ondersteunt het wachtwoord instellen
    - Ondersteunt geen wachtwoord wijzigen
- Oracle (eerder zondag) Directory Server Enterprise Edition
    - Ondersteuning biedt voor alle bewerkingen voor delta importeren
    - Ondersteunt het wachtwoord en wachtwoord wijzigen instellen
- RadiantOne virtuele map Server (VDS)
    - Versie 7.1.1 of hoger
    - Ondersteuning biedt voor alle bewerkingen voor delta importeren
    - Ondersteunt het wachtwoord en wachtwoord wijzigen instellen
-  Zo een Directory-Server
    - Ondersteuning biedt voor alle bewerkingen voor delta importeren
    - Ondersteunt het wachtwoord en wachtwoord wijzigen instellen

### <a name="prerequisites"></a>Vereisten voor
Voordat u de verbindingslijn gebruiken, te controleren of u de volgende handelingen uit op de synchronisatieserver:

- 4.5.2 van Microsoft .NET Framework of hoger

### <a name="detecting-the-ldap-server"></a>Detectie van de LDAP-server
De verbindingslijn is afhankelijk van bladwijzers opsporen en uw de LDAP-server. De verbindingslijn wordt gebruikt voor de hoofdmap DSE, leverancier naam/versie en zij keurt het schema om unieke objecten en kenmerken bekend in bepaalde LDAP-servers te zoeken. Deze gegevens als gevonden, wordt de configuratieopties in Connector vooraf in te vullen.

### <a name="connected-data-source-permissions"></a>Verbonden gegevensbron machtigingen
Als u wilt importeren en exporteren van de objecten in de adreslijst verbonden, moet de connector-account voldoende machtigingen hebben. De verbindingslijn moet schrijfmachtigingen moeten kunnen exporteren en leesmachtigingen kunnen importeren. Configuratie van de machtiging wordt uitgevoerd binnen de ervaringen beheer van de doelmap zelf.

### <a name="ports-and-protocols"></a>Poorten en protocollen
De verbindingslijn wordt gebruikt voor het poortnummer die is opgegeven in de configuratie, die standaard 389 voor LDAP en 636 voor leesbaar TEKSTFORMAAT is.

Voor leesbaar TEKSTFORMAAT, moet u SSL 3.0 of TLS. SSL 2.0 wordt niet ondersteund en kan niet worden geactiveerd.

### <a name="required-controls-and-features"></a>Vereiste besturingselementen en -functies
De volgende LDAP-besturingselementen/functies moet zijn beschikbaar op de LDAP-server voor de verbindingslijn werkt alleen naar behoren:  
`1.3.6.1.4.1.4203.1.5.3`Waar/onwaar filters

Het filter waar/onwaar vaak niet wordt vermeld ondersteund door de LDAP-mappen en mogelijk wordt weergegeven op de **Globale pagina** onder **Verplicht functies niet gevonden**. Wordt gebruikt **of** filters maken in LDAP-query's, bijvoorbeeld bij het importeren van meerdere objecttypen. Als u meer dan één objecttype importeren kunt, klikt u vervolgens uw LDAP-server deze functie wordt ondersteund.

Als u een map waarbij een unieke id is het anker moet de volgende ook beschikbaar (Zie de sectie [Configureren ankers](#configure-anchors) verderop in dit artikel voor meer informatie):  
`1.3.6.1.4.1.4203.1.5.1`Alle operationele kenmerken

Als de map meer objecten bevat dan in een oproep door naar de map passen, wordt klikt u vervolgens het aanbevolen paginering gebruiken. Voor paginering om te werken, moet u een van de volgende opties:

**Optie 1:**  
`1.2.840.113556.1.4.319`pagedResultsControl

**Optie 2:**  
`2.16.840.1.113730.3.4.9`VLVControl  
`1.2.840.113556.1.4.473`SortControl

Als beide opties zijn ingeschakeld in de configuratie van de connector, wordt pagedResultsControl gebruikt.

`1.2.840.113556.1.4.417`ShowDeletedControl

ShowDeletedControl wordt alleen gebruikt met de methode USNChanged delta import moeten kunnen zien verwijderde objecten.

De verbindingslijn wordt gezocht naar de opties die aanwezig zijn op de server. Als de opties niet kunnen worden gevonden, wordt een waarschuwing aanwezig is op de globale pagina in de eigenschappen van de verbindingslijn is. Niet alle LDAP-servers presenteren alle besturingselementen/functies die worden ondersteund en zelfs als deze waarschuwing aanwezig is, de verbindingslijn werken mogelijk zonder problemen.

### <a name="delta-import"></a>Delta importeren
Delta importeren is alleen beschikbaar wanneer u een map ondersteuning is gevonden. De volgende manieren worden momenteel gebruikt:

- LDAP-Accesslog. Zie [http://www.openldap.org/doc/admin24/overlays.html#Access vastleggen](http://www.openldap.org/doc/admin24/overlays.html#Access Logging)
- LDAP-Changelog. Zie [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
- Tijdstempel. De verbindingslijn wordt voor Novell/NetIQ eDirectory, laatste datum/tijd gebruikt om gemaakt en bijgewerkt objecten. Novell/NetIQ eDirectory biedt geen equivalent betekent dat verwijderde objecten ophalen. Deze optie kan ook worden gebruikt als er geen andere delta importmethode actief op de LDAP-server. Deze optie is niet mogen objecten importeren die zijn verwijderd.
- USNChanged. Zie: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Niet ondersteund
De volgende LDAP-functies worden niet ondersteund:

- LDAP-verwijzingen tussen servers (RFC 4511/4.1.10)

## <a name="create-a-new-connector"></a>Een nieuwe verbindingslijn maken
Selecteer een algemene LDAP om connector te maken, in **Synchronisatieservice** **Management Agent** en **maken**. Selecteer de verbindingslijn **algemene LDAP (Microsoft)** .

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Connectiviteit
Klik op de pagina Connectivity moet u de gegevens Host, poorten en Binding. Afhankelijk van welke Binding ingeschakeld, aanvullende is mogelijk informatie in de volgende secties worden opgegeven.

![Connectiviteit](./media/active-directory-aadconnectsync-connector-genericldap/connectivity.png)

- De instelling time-out van de verbinding wordt alleen gebruikt voor de eerste verbinding met de server wanneer het schema opsporen.
- Als Binding anoniem, klikt u vervolgens geen van beide gebruikersnaam / wachtwoord noch certificaat worden gebruikt.
- Voer voor andere bindingen, gegevens beide in gebruikersnaam / wachtwoord of Selecteer een certificaat.
- Als u Kerberos gebruikt om te verifiëren, klikt u vervolgens Daarnaast bieden de Realm of het domein van de gebruiker.

Het **kenmerk aliassen** tekstvak wordt gebruikt voor kenmerken die zijn gedefinieerd in het schema met de syntaxis van de RFC4522. Deze kenmerken kunnen niet worden gedetecteerd tijdens de schema detectie en de verbindingslijn moet worden geïdentificeerd die kenmerken. De volgende is bijvoorbeeld nodig om te worden ingevoerd in het vak kenmerk aliassen te herkennen het kenmerk Eigenschapsspecifiek als een binaire kenmerk:

`userCertificate;binary`

Hier volgt een voorbeeld van hoe deze configuratie ziet als eruit:

![Connectiviteit](./media/active-directory-aadconnectsync-connector-genericldap/connectivityattributes.png)

Schakel het selectievakje **operationele kenmerken in schema opnemen** ook kenmerken die zijn gemaakt door de server opnemen. Hierbij kenmerken, zoals wanneer het object is gemaakt en datum van laatste update.

Selecteer **extensible kenmerken opnemen in schema** als extensible objecten (RFC4512/4.3) worden gebruikt en u deze optie inschakelt, elk kenmerk kunt moet worden gebruikt voor alle object. Deze optie zorgt ervoor dat het schema zeer grote tenzij de verbonden map is deze functie gebruikt de aanbeveling wordt de optie niet behouden.

### <a name="global-parameters"></a>Globale Parameters
Op de pagina globale Parameters configureert u de DN-naam voor de wijzigingenlogboek delta en aanvullende LDAP-functies. De pagina is ingevuld met de informatie van de LDAP-server.

![Connectiviteit](./media/active-directory-aadconnectsync-connector-genericldap/globalparameters.png)

De bovenste sectie bevat informatie verstrekt door de server zelf, zoals de naam van de server. De verbindingslijn controleert ook dat de verplicht besturingselementen aanwezig in de hoofdmap DSE zijn. Als deze besturingselementen niet weergegeven worden, wordt een waarschuwing weergegeven. Een aantal LDAP-mappen niet alle functies in de hoofdmap DSE aanbieden en is het mogelijk dat de verbindingslijn altijd zonder problemen, werkt zelfs als een waarschuwing aanwezig is.

Het gedrag voor bepaalde bewerkingen bepaalt als volgt de selectievakjes **besturingselementen ondersteund** :

- Met de structuurweergave verwijderen is geselecteerd, een hiërarchie wordt verwijderd met één LDAP-oproep. De verbindingslijn biedt een recursieve verwijderen met de structuurweergave verwijderen is uitgeschakeld, indien nodig.
- Met verwisselbaar resultaten is geselecteerd, wordt de verbindingslijn een verwisselbaar importeren met de grootte die is opgegeven voor de stappen uitvoeren.
- De VLVControl en SortControl is een alternatief voor het pagedResultsControl te lezen van gegevens uit de LDAP-diagram.
- Als alle drie de opties (pagedResultsControl, VLVControl en SortControl), niet ingeschakeld zijn en vervolgens de verbindingslijn alle objecten in één bewerking mislukt mogelijk als het een grote map is geïmporteerd.
- ShowDeletedControl wordt alleen gebruikt wanneer de methode Delta import USNChanged is.

Het wijzigingenlogboek DN is de context voor de naam die wordt gebruikt door het wijzigingenlogboek delta bijvoorbeeld **cn = changelog**. Deze waarde moet worden opgegeven om te kunnen doen delta importeren.

Hier volgt een lijst met wijzigingenlogboek standaard DNs:

Directory | Wijzigingenlogboek delta
--- | ---
Microsoft AD LDS en AD ° c | Automatisch gedetecteerd. USNChanged.
Apache Directory-Server | Niet beschikbaar.
Directory 389 | Wijzigingenlogboek. Standaardwaarde te gebruiken: **cn = changelog**
IBM Tivoli DS | Wijzigingenlogboek. Standaardwaarde te gebruiken: **cn = changelog**
Isode Directory | Wijzigingenlogboek. Standaardwaarde te gebruiken: **cn = changelog**
Novell/NetIQ eDirectory | Niet beschikbaar. Tijdstempel. De verbindingslijn doeleinden laatst bijgewerkt datum/tijd om toegevoegd en de records.
Open DJ/DS | Wijzigingenlogboek.  Standaardwaarde te gebruiken: **cn = changelog**
Open LDAP | Access-logboek. Standaardwaarde te gebruiken: **cn = accesslog**
Oracle DSEE | Wijzigingenlogboek. Standaardwaarde te gebruiken: **cn = changelog**
RadiantOne VDS | Virtuele map. Is afhankelijk van de map die is verbonden met VDS.
Zo een Directory-Server | Wijzigingenlogboek. Standaardwaarde te gebruiken: **cn = changelog**

Het wachtwoord-kenmerk is de naam van het kenmerk dat de verbindingslijn gebruiken moet voor het instellen van het wachtwoord in wachtwoord wijzigen en wachtwoord ingesteld bewerkingen.
Deze waarde is standaard ingesteld op **userPassword** , maar kan worden gewijzigd wanneer u nodig hebt voor een bepaald LDAP-systeem.

Klik in de lijst extra partities is het mogelijk om toe te voegen extra naamruimten niet automatisch gedetecteerd. Deze instelling kan bijvoorbeeld worden gebruikt als verschillende servers maakt u een logische cluster, dat moet worden alle geïmporteerd op hetzelfde moment. Net zoals Active Directory meerdere domeinen in één bos hebben kunt, maar alle domeinen één schema delen, kunt u hetzelfde gesimuleerd door in te voeren van de aanvullende naamruimten in dit vak. Elke naamruimte kunt importeren uit verschillende servers en verder is geconfigureerd op de pagina partities configureren en hiërarchieën. Gebruik Ctrl + Enter om een nieuwe regel.

### <a name="configure-provisioning-hierarchy"></a>Inrichten hiërarchie configureren
Deze pagina wordt gebruikt voor de DN-component, bijvoorbeeld OU toewijzen aan het objecttype dat moet worden deze is ingericht, bijvoorbeeld organizationalUnit.

![Hiërarchie inrichting](./media/active-directory-aadconnectsync-connector-genericldap/provisioninghierarchy.png)

U configureert inrichten hiërarchie, kunt u de verbindingslijn automatisch maken van een structuur als dat nodig is. Bijvoorbeeld = contoso, als er een domeincontroller naamruimte domeincontroller = com en een nieuw object cn = Jan, ou = Seattle, c = US, domeincontroller = contoso, domeincontroller = com is ingericht en klik op de verbindingslijn kunt een object van het type land voor de Verenigde Staten en een organizationalUnit voor Seattle maken als die nog niet aanwezig zijn in de adreslijst.

### <a name="configure-partitions-and-hierarchies"></a>Partities en hiërarchieën configureren
Selecteer alle naamruimten met objecten die u wilt importeren en exporteren op de pagina partities en hiërarchieën.

![Partities](./media/active-directory-aadconnectsync-connector-genericldap/partitions.png)

Voor elke naamruimte is het ook mogelijk te configureren verbindingsinstellingen die de waarden die zijn opgegeven in het scherm Connectivity wilt overschrijven. Als deze waarden naar de standaardwaarde van een leeg gelaten worden, worden de gegevens in het scherm Connectivity worden gebruikt.

Het is ook mogelijk om welke containers en organisatie-eenheden de verbindingslijn moet uit importeren en exporteren naar te selecteren.

### <a name="configure-anchors"></a>Ankers configureren
Deze pagina altijd een vooraf geconfigureerde waarde en kan niet worden gewijzigd. Als de leverancier van de server is geïdentificeerd, kan het anker worden gevuld met een onveranderlijke kenmerk is, bijvoorbeeld de GUID voor een object. Als dit is niet gedetecteerd of u wilt voorkomen dat een kenmerk onveranderlijke bekend is, wordt de verbindingslijn dn (distinguished name) als het anker.

![ankers](./media/active-directory-aadconnectsync-connector-genericldap/anchors.png)

Hier volgt een lijst met LDAP-servers en het anker dat wordt gebruikt:

Directory | Ankerkenmerk
--- | ---
Microsoft AD LDS en AD ° c | objectGUID
389 directory-Server | DN-naam
Apache Directory | DN-naam
IBM Tivoli DS | DN-naam
Isode Directory | DN-naam
Novell/NetIQ eDirectory | GUID
Open DJ/DS | DN-naam
Open LDAP | DN-naam
Oracle ODSEE | DN-naam
RadiantOne VDS | DN-naam
Zo een Directory-Server | DN-naam

## <a name="other-notes"></a>Andere notities
In dit gedeelte vindt u informatie van aspecten die specifiek zijn voor deze Connector of om een andere reden zijn belangrijk.

### <a name="delta-import"></a>Delta importeren
Het watermerk delta in geopende LDAP is UTC datum/tijd. Daarom moeten de klok tussen FIM-synchronisatieservice en de geopende LDAP worden gesynchroniseerd. Als dit niet het geval is, wordt mogelijk worden sommige items in het wijzigingenlogboek delta weggelaten.

Het importeren delta detecteert voor Novell eDirectory, geen een object verwijderen. Om die reden is het moet u een volledige importbewerking regelmatig om te zoeken naar alle verwijderde objecten uitvoeren.

Voor mappen met een delta wijzigingenlogboek die is gebaseerd op datum/tijd en is het raadzaam een volledige importbewerking uitvoeren op periodieke momenten. Deze procedure kunt u de synchronisatie-engine om te zoeken en samenstellingstraject tussen de LDAP-server en wat is momenteel in de ruimte verbindingslijn.

## <a name="troubleshooting"></a>Problemen oplossen

-   Zie voor informatie over het inschakelen van logboekregistratie om op te lossen de verbindingslijn, [hoe u ETW tracering inschakelen voor verbindingslijnen](http://go.microsoft.com/fwlink/?LinkId=335731).
