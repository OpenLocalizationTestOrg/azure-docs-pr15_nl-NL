<properties
    pageTitle="Synchronisatie van Azure AD Connect: lidmaatschap declaratieve inrichting | Microsoft Azure"
    description="Dit artikel wordt uitgelegd het declaratieve inrichten configuratiemodel in Azure AD Connect."
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
    ms.date="08/29/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-understanding-declarative-provisioning"></a>Azure AD Connect synchroniseren: wat declaratieve inrichting
In dit onderwerp wordt uitgelegd dat het configuratiemodel in Azure AD Connect. Het model heet declaratieve inrichting en deze kunt u een configuratie gemakkelijk wijzigen. Veel zaken die worden beschreven in dit onderwerp zijn geavanceerde en niet vereist voor de meeste klant-scenario's.

## <a name="overview"></a>Overzicht
Declaratieve inrichting objecten die afkomstig zijn uit een bron verbonden map wordt verwerkt, en bepaalt hoe het object en kenmerken moeten worden omgezet van een bron naar een doelsite. Een object wordt verwerkt in een pijplijn synchroniseren en de pijplijn werkt op dezelfde regels voor binnenkomende en uitgaande. Een binnenkomende regel is een spatie verbindingslijn tot de metaverse en een uitgaande regel is van de metaverse naar een spatie verbindingslijn.

![Synchronisatie verkooppijplijn](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/sync1.png)  

De pijplijn heeft verschillende verschillende modules. Elk is verantwoordelijk voor één concept in object-synchronisatie.

![Synchronisatie verkooppijplijn](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/pipeline.png)  

- Bron, het bronobject
- [Reikwijdte](#scope), vindt u alle regels voor synchronisatie die in een bereik
- [Deelnemen aan](#join), bepaald relatie tussen verbindingslijn ruimte en metaverse
- [Transformeren](#transform), berekent hoe kenmerken moeten worden omgezet en doorlopen
- [De prioriteit van regels](#precedence), lost u conflicterende kenmerk bijdragen aan goede doelen
- Doel, het doelobject

## <a name="scope"></a>Bereik
De module bereik is evaluatie van een object en de regels die zich bevinden in omvang en moeten worden opgenomen in de verwerking bepaalt. Afhankelijk van de waarden van kenmerken op het object, worden verschillende synchronisatie-regels worden geëvalueerd in het bereik. Een uitgeschakelde gebruiker die geen Exchange-postvak heeft bijvoorbeeld andere regels dan een ingeschakelde-gebruiker met een postvak.  
![Bereik](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope1.png)  

Het bereik is gedefinieerd als groepen en componenten. De clausules zijn in een groep. Een logische en wordt gebruikt tussen alle componenten in een groep. Bijvoorbeeld (afdeling IT en land = = Denemarken). Een logische OR wordt gebruikt tussen de groepen.

![Bereik](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope2.png)  
Als het bereik in deze afbeelding worden gelezen (afdeling IT en land = = Denemarken) of (land = Zweden). Als groep 1 of groep 2 is geëvalueerd als true, wordt de regel is in het bereik.

De module bereik ondersteunt de volgende bewerkingen.

Bewerking | Beschrijving
--- | ---
GELIJK, NOTEQUAL | Het vergelijken van een tekenreeks die wordt geëvalueerd als waarde gelijk aan de waarde in het kenmerk is. Zie voor meerdere waarden kenmerken, ISIN en ISNOTIN.
LESSTHAN, LESSTHAN_OR_EQUAL | Een tekenreeks vergelijken die wordt geëvalueerd als waarde is minder dan van de waarde in het kenmerk.
BEVAT, NOTCONTAINS | Het vergelijken van een tekenreeks die wordt geëvalueerd als waarde u ergens in de waarde in het kenmerk vindt.
STARTSWITH, NOTSTARTSWITH | Het vergelijken van een tekenreeks die wordt geëvalueerd als waarde in het begin van de waarde in het kenmerk.
ENDSWITH, NOTENDSWITH | Het vergelijken van een tekenreeks die wordt geëvalueerd als waarde in het einde van de waarde in het kenmerk.
GROTER DAN, GREATERTHAN_OR_EQUAL | Het vergelijken van een tekenreeks die wordt geëvalueerd als waarde groter dan van de waarde in het kenmerk is.
ISNULL, ISNOTNULL | Als het kenmerk ontbreekt evalueert van het object. Als het kenmerk niet aanwezig en kunnen daarom null is, klikt u vervolgens is de regel in een bereik.
ISIN, ISNOTIN | Als de waarde in het gedefinieerde kenmerk is evalueert. Deze bewerking is de met meerdere waarden variatie gelijk en NOTEQUAL. Het kenmerk moet een kenmerk met meerdere waarden zijn en als de waarde u in een van de waarden van het kenmerk vindt, klikt u vervolgens de regel staat in het bereik.
ISBITSET, ISNOTBITSET | Evalueert als een bepaalde bit is ingesteld. Bijvoorbeeld kan worden gebruikt voor de evaluatie van de bits in userAccountControl om te zien als een gebruiker is ingeschakeld of uitgeschakeld.
ISMEMBEROF, ISNOTMEMBEROF | De waarde moet een DN aan een groep in de ruimte verbindingslijn bevatten. Als het object een lid van de groep die is opgegeven is, wordt de regel in een bereik is.

## <a name="join"></a>Deelnemen aan
De module join in de pijplijn synchronisatie is verantwoordelijk voor het vinden van de relatie tussen het object in de bron en een object in de doellijst. Klik op een binnenkomende regel zou deze relatie een object in een verbindingslijn ruimte zoeken van een relatie aan een object in de metaverse.  
![Deelnemen aan tussen cs en mv](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join1.png)  
Het doel is om te zien dat als er nog een object in de metaverse, gemaakt door een andere Connector, moet deze worden gekoppeld. In een account-resource bos moet de gebruiker in het account bos bijvoorbeeld worden samengevoegd met de gebruiker van de resource bos.

Joins worden gebruikt voornamelijk op regels voor binnenkomende verbindingen om verbindingslijn ruimteobjecten samen te voegen aan hetzelfde object metaverse.

De joins worden gedefinieerd als een of meer groepen. U hebt componenten binnen een groep. Een logische en wordt gebruikt tussen alle componenten in een groep. Een logische OR wordt gebruikt tussen groepen. De groepen worden verwerkt in volgorde van boven naar beneden. Als één groep precies één vergelijken met een object in de doellijst gevonden heeft, worden klikt u vervolgens geen andere join regels geëvalueerd. Als nul of meer dan één object wordt gevonden, blijft verwerking van de volgende groep met regels. Daarom moet de regels in de volgorde van de meeste expliciete eerste gemaakt en meer bij benadering aan het einde.  
![Deelnemen aan de definitie](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join2.png)  
De joins in deze afbeelding worden verwerkt van boven naar beneden. Eerst ziet de synchronisatie-pijplijn als er een overeenkomst op het veld Werknemer-id. Als dit niet het geval is, wordt de tweede regel ziet als de naam van het account kan worden gebruikt om de objecten samen te voegen. Als dat niet geschikt is al, is de derde en laatste regel een meer onduidelijk vergelijken met behulp van de naam van de gebruiker.

Als alle regels voor join hebt is geëvalueerd en er niet precies één van de zoekwaarde is, wordt het **Type koppeling** op de pagina **Beschrijving** wordt gebruikt. Als deze optie is ingesteld **inrichten**, wordt klikt u vervolgens een nieuw object in de doellijst gemaakt.  
![Voorziening of eraan deelnemen](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join3.png)  

Een object mag alleen een synchronisatie van één regel met regels van de join in reikwijdte hebben. Als er meerdere regels voor synchronisatie, waarbij de join is gedefinieerd, wordt een fout optreedt. De prioriteit van regels niet gebruikt voor het deelnemen aan conflicten oplossen. Een object moet een join-regel in het bereik voor kenmerken naar flow met dezelfde inkomende/uitgaande poort richting. Als u stroom kenmerken binnenkomende en uitgaande op hetzelfde object moet, moet u een inkomende en een regel voor het uitgaande synchroniseren met join hebben.

Uitgaande join heeft een speciaal gedrag wanneer wordt geprobeerd voor het inrichten van een object naar een doel verbindingslijn spatie. Het kenmerk DN wordt gebruikt voor het eerst probeert een reverse-join. Als er bestaat al een object in de ruimte doel verbindingslijn met de dezelfde DN door zijn de objecten gekoppeld.

De join-module alleen geëvalueerd wanneer wanneer een nieuwe regel voor het synchroniseren in de scope wordt geleverd. Wanneer een object heeft die zijn gekoppeld, is deze niet verwijderen zelfs als de join-criteria niet meer wordt voldaan. Als u een object Meld wilt, wordt de synchronisatie-regel die deelneemt aan de objecten moet gaan buiten het bereik.

### <a name="metaverse-delete"></a>Metaverse verwijderen
Een object metaverse blijft staan als lang als er één synchroniseren in een bereik met **Koppelingstype** regelset **inrichten** of **StickyJoin**. Een StickyJoin wordt gebruikt wanneer een verbindingslijn is niet toegestaan voor het inrichten van een nieuw object naar de metaverse, maar wanneer deze heeft die zijn gekoppeld, kan deze moet worden verwijderd in de bron voordat het object metaverse wordt verwijderd.

Wanneer een object metaverse wordt verwijderd, worden alle objecten die zijn gekoppeld aan een regel voor het uitgaande synchronisatie gemarkeerd voor het **inrichten van** zijn gemarkeerd voor een verwijderen.

## <a name="transformations"></a>Transformaties
De transformaties worden gebruikt om te bepalen hoe kenmerken moeten stromen uit de bron naar de doelsite. De loopt kunnen beschikken over een van de volgende **stroomtypen**: Direct constante of expressie. Een rechtstreekse stroom, loopt een kenmerkwaarde als-is met geen aanvullende transformaties in. Een constante waarde Hiermee stelt u de opgegeven waarde. Een expressie wordt de declaratieve inrichten expressietaal Express hoe de transformatie moet worden gebruikt. De details in voor de expressietaal vindt u in het onderwerp [lidmaatschap declaratieve inrichten expressietaal](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) .

![Voorziening of eraan deelnemen](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/transformations1.png)  

Het selectievakje **eenmaal toepassen** Hiermee definieert u dat het kenmerk moet alleen worden ingesteld als het object voor het eerst wordt gemaakt. Deze configuratie kan bijvoorbeeld worden gebruikt voor het instellen van een eerste wachtwoord voor een nieuwe gebruikersobject.

### <a name="merging-attribute-values"></a>Kenmerkwaarden samenvoegen
Er is een instelling om te bepalen als meerdere waarden kenmerken vanuit verschillende verschillende verbindingslijnen moeten worden samengevoegd in de loopt kenmerk. De standaardwaarde is **Update**, die wordt aangegeven dat de synchronisatie-regel met de hoogste prioriteit moet winnen.

![Typen samenvoegen](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/mergetype.png)  

Er is ook **samenvoegen** en **MergeCaseInsensitive**. Deze opties kunt u waarden uit verschillende gegevensbronnen samenvoegen. Dit kan bijvoorbeeld worden gebruikt om samen te voegen van het lid dat of de proxyAddresses-kenmerk uit diverse verschillende forests. Wanneer u deze optie gebruikt, moeten hetzelfde type samenvoegen gebruiken in alle regels voor synchronisatie in het bereik voor een object. U kunt geen **Update** uit één verbindingslijn en **samenvoegen** van elkaar aftrekken definiëren. Als u probeert, ontvangt u een fout.

Het verschil tussen **samenvoegen** en **MergeCaseInsensitive** wordt uitgelegd hoe dubbele waarden verwerken. De synchronisatie-engine zorgt ervoor dubbele waarden niet worden ingevoegd in de doelkenmerk. Met **MergeCaseInsensitive**, dubbele waarden met alleen een verschil geval kunnen niet worden verzonden naar aanwezig zijn. Zoals u beide niet mogen zien "SMTP:bob@contoso.com" en "smtp:bob@contoso.com" in de doelkenmerk. **Samenvoegen** is alleen bekijken via de exacte waarden en meerdere waarden waarbij alleen is er een verschil in hoofdletters/kleine letters mogelijk presenteren.

De optie **vervangen** is hetzelfde als de **Update**, maar deze niet worden gebruikt.

### <a name="control-the-attribute-flow-process"></a>Het kenmerk stroom-proces beheren
Als meerdere regels voor binnenkomende synchronisatie zijn geconfigureerd voor bijdragen aan hetzelfde metaverse kenmerk, wordt de prioriteit van regels gebruikt om te bepalen de winnaar. De synchronisatie-regel met de hoogste prioriteit (laagste numerieke waarde) dient bijdragen de waarde. Hetzelfde gebeurt voor uitgaande regels. De synchronisatie met de hoogste prioriteit van regels voor wins regel en de waarde voor de verbonden adreslijst bijdragen.

In sommige gevallen, in plaats van een waarde, bijdragen moet de synchronisatie-regel bepalen hoe andere regels moeten werken. Er zijn enkele speciale letterlijke waarden voor deze zaak.

Voor binnenkomende synchronisatieregels kan om aan te geven dat de stroom geen waarde heeft bijdragen de letterlijke **NULL** worden gebruikt. Een andere regel met een lagere prioriteit kan een waarde bijdragen. Als er geen regel een waarde bijgedragen, wordt het kenmerk metaverse verwijderd. Voor een uitgaande regel als **NULL** de laatste waarde is nadat alle regels voor synchronisatie zijn verwerkt, wordt klikt u vervolgens de waarde verwijderd in de verbonden adreslijst.

De letterlijke **AuthoritativeNull** lijkt op **NULL** , maar met het verschil dat geen regels met lagere prioriteit van regels voor een waarde kunnen bijdragen.

Een stroom kenmerk kunt ook **IgnoreThisFlow**gebruiken. Dit is vergelijkbaar met Null-waarden in de zin dat betekent dit er niets dat bijdragen. Het verschil is dat wordt niet verwijderd een al bestaande waarde in de doellijst. Het lijkt, de stroom kenmerk is nooit er.

Hier volgt een voorbeeld:

In het *Out naar AD - hybride implementatie van Exchange van de gebruiker* kunt de volgende stroom vinden:  
`IIF([cloudSOAExchMailbox] = True,[cloudMSExchSafeSendersHash],IgnoreThisFlow)`  
Deze expressie moet worden gelezen als: als het gebruikerspostvak in Azure AD bevindt zich, klikt u vervolgens het kenmerk van Azure AD-AD flow. Als dit niet het geval is, wordt terug naar Active Directory niet flow. In dit geval zou deze de huidige waarde in AD behouden.

### <a name="importedvalue"></a>ImportedValue
De functie ImportedValue is anders dan alle andere functies, omdat de naam van het kenmerk moet tussen aanhalingstekens in plaats van vierkante haken:  
`ImportedValue("proxyAddresses")`.

Meestal tijdens de synchronisatie van een kenmerk wordt de verwachte waarde, gebruikt, zelfs als deze nog niet is zijn geëxporteerd of een fout is ontvangen tijdens het exporteren ("boven aan de tower"). Een inkomende synchronisatie wordt ervan uitgegaan dat een kenmerk dat nog niet is bereikt een verbonden directory uiteindelijk deze bereikt. In sommige gevallen is het belangrijk om te synchroniseren alleen een waarde die is bevestigd door de verbonden adreslijst ('hologram en delta tower importeren').

Een voorbeeld van deze functie kunt u vinden in de regel van de synchronisatie kant-en-klare *In uit Active Directory-gebruiker algemene van Exchange*. In hybride Exchange, moet de waarde die is toegevoegd door Exchange online alleen worden gesynchroniseerd wanneer deze is bevestigd dat de waarde is geëxporteerd:  
`proxyAddresses` <- `RemoveDuplicates(Trim(ImportedValue("proxyAddresses")))`

## <a name="precedence"></a>De prioriteit van regels
Wanneer meerdere regels voor synchronisatie bijdragen de dezelfde kenmerkwaarde naar de doelsite wilt, wordt de waarde van de prioriteit van regels voor wordt gebruikt om te bepalen de winnaar. De regel met de hoogste prioriteit, laagste numerieke waarde is, dient het kenmerk in een conflict bijdragen.

![Typen samenvoegen](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/precedence1.png)  

Deze volgorde kan worden gebruikt om te definiëren nauwkeuriger kenmerk stromen voor een klein aantal objecten. Bijvoorbeeld zorgen de out-van-vak-regels dat de kenmerken van een ingeschakelde (**Gebruiker AccountEnabled**)-account de prioriteit van regels uit andere accounts hebben.

De prioriteit van regels kan worden gedefinieerd tussen verbindingslijnen. Waarmee verbindingslijnen met betere gegevens waarden eerst bijdragen.

### <a name="multiple-objects-from-the-same-connector-space"></a>Meerdere objecten uit dezelfde verbindingslijn ruimte
Als u meerdere objecten in dezelfde verbindingslijn ruimte die zijn gekoppeld aan hetzelfde object metaverse hebt, moet de prioriteit van regels worden aangepast. De synchronisatie-engine is niet kunnen bepalen van de prioriteit van regels als verschillende objecten in het bereik van dezelfde regel voor synchronisatie. Het is niet-eenduidige welke bronobject moet de waarde moet worden de metaverse bijdragen. Deze configuratie wordt vermeld als niet-eenduidige, zelfs als de kenmerken in de bron dezelfde waarde hebben.  
![Meerdere objecten die zijn gekoppeld aan hetzelfde object mv](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple1.png)  

In dit scenario moet u het bereik van de synchronisatie-regels wijzigen zodat de bronobjecten verschillende synchronisatie regels in het bereik hebben. Waarmee u verschillende prioriteit van regels definiëren.  
![Meerdere objecten die zijn gekoppeld aan hetzelfde object mv](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple2.png)  

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de expressietaal in [Lidmaatschap declaratieve inrichting van expressies](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
- Zie hoe declaratieve inrichting gebruikte kant-en-klare begrijpen van [de standaardconfiguratie](active-directory-aadconnectsync-understanding-default-configuration.md)is.
- Lees hoe u een praktische wijziging met declaratieve inrichting in [hoe u een wijziging in de standaardconfiguratie](active-directory-aadconnectsync-change-the-configuration.md)aanbrengen.
- Gaat u verder te lezen hoe gebruikers en contactpersonen in [wat gebruikers en contactpersonen samenwerken](active-directory-aadconnectsync-understanding-users-and-contacts.md).

**Van overzichtsonderwerpen**

- [Synchronisatie van Azure AD Connect: begrijpen en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
- [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)

**Naslaginformatie**

- [Synchronisatie van Azure AD Connect: overzicht van functies](active-directory-aadconnectsync-functions-reference.md)
