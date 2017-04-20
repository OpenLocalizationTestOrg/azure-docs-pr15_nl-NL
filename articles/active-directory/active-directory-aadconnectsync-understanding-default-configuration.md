<properties
    pageTitle="Synchronisatie van Azure AD Connect: informatie over de standaardconfiguratie | Microsoft Azure"
    description="In dit artikel worden de standaard-configuratie in Azure AD Connect synchroniseren."
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
    ms.date="09/01/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-sync-understanding-the-default-configuration"></a>Synchronisatie van Azure AD Connect: informatie over de standaardconfiguratie
In dit artikel wordt uitgelegd dat de van de kant-en-klare configuratieregels. Deze documenten de regels en de invloed van deze regels op de configuratie. Ook begeleidt u bij de standaardconfiguratie van Azure AD Connect synchroniseren. Het doel is dat de lezer hoe het configuratiemodel begrijpt, met de naam declaratieve is geïnstalleerd, werkt in een echte voorbeeld. In dit artikel wordt ervan uitgegaan dat u al hebt geïnstalleerd en configureren van Azure AD Connect synchroniseren met de wizard installeren.

Als u wilt weten over de details van het configuratiemodel, Lees [Wat declaratieve inrichten](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

## <a name="out-of-box-rules-from-on-premises-to-azure-ad"></a>Kant-en-klare regels vanuit on-premises Azure AD
De volgende expressies vindt u in de configuratie kant-en-klare.

### <a name="user-out-of-box-rules"></a>Gebruiker kant-en-klare regels
Deze regels worden ook toegepast op het objecttype iNetOrgPerson.

Een gebruikersobject moet voldoen aan de volgende handelingen uit die kan worden gedownload:

- Moet u een sourceAnchor.
- Nadat het object is gemaakt in Azure AD, klikt u vervolgens wijzigen sourceAnchor niet. Als de waarde gewijzigde on-premises implementatie is, het object drukt, stopt synchroniseren totdat de sourceAnchor terug naar de vorige waarde is gewijzigd.
- Moet hebben het kenmerk accountEnabled (userAccountControl) zijn ingevuld. Met een lokale Active Directory is dit kenmerk altijd presenteren en gevulde.

De volgende gebruikersobjecten worden **niet** gesynchroniseerd met Azure AD:

- `IsPresent([isCriticalSystemObject])`. Zorgen dat u veel kant-en-klare objecten in Active Directory, zoals het ingebouwde beheerdersaccount, worden niet gesynchroniseerd.
- `IsPresent([sAMAccountName]) = False`. Controleer of gebruikersobjecten zonder sAMAccountName-kenmerk worden niet gesynchroniseerd. In dit geval gebeurt alleen praktisch in een domein bijgewerkt van NT 4.
- `Left([sAMAccountName], 4) = "AAD_"`, `Left([sAMAccountName], 5) = "MSOL_"`. De serviceaccount gebruikt door Azure AD Connect-synchronisatie en de bijbehorende eerdere versies worden niet worden gesynchroniseerd.
- Exchange-accounts die niet in Exchange Online werkt, worden niet worden gesynchroniseerd.
    - `[sAMAccountName] = "SUPPORT_388945a0"`
    - `Left([mailNickname], 14) = "SystemMailbox{"`
    - `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`
    - `(Left([sAMAccountName], 4) = "CAS_" && (InStr([sAMAccountName], "}")> 0))`
- Objecten die niet in Exchange Online werkt, worden niet worden gesynchroniseerd.
`CBool(IIF(IsPresent([msExchRecipientTypeDetails]),BitAnd([msExchRecipientTypeDetails],&H21C07000) > 0,NULL))`  
In dit bitmasker (& H21C07000) zou uitfilteren aan de volgende objecten:
    - Openbare map e-mailadres
    - Systeem Attendant postvak
    - Postvak Database postvak (systeempostvak)
    - Universele beveiligingsgroep (wouldn't toepassen voor een gebruiker, maar vanwege de oudere aanwezig is)
    - Niet-universele groep (wouldn't toepassen voor een gebruiker, maar vanwege de oudere aanwezig is)
    - Postvak-abonnement
    - Detectiepostvak
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Replicatie slachtoffer objecten worden niet worden gesynchroniseerd.

De volgende kenmerk regels van toepassing:

- `sourceAnchor <- IIF([msExchRecipientTypeDetails]=2,NULL,..)`. Een gekoppelde postvak is niet het kenmerk sourceAnchor bijgedragen. Dat de werkelijke account als later is gekoppeld als een gekoppelde postvak is gevonden, wordt uitgegaan.
- Exchange gerelateerde kenmerken worden alleen gesynchroniseerd als het kenmerk **mailNickName** een waarde heeft.
- Wanneer er meerdere forests, worden kenmerken verbruikt in de volgende volgorde:
    1. De bos met een ingeschakelde account zijn gerelateerd aan aanmeldingsproblemen kenmerken (bijvoorbeeld userPrincipalName) bijgedragen.
    2. Kenmerken die u in een GAL Exchange (algemene adreslijst vindt) zijn bijgedragen de bos met een Exchange-postvak.
    3. Als er geen postvak kan worden gevonden, klikt u vervolgens deze kenmerken kunnen afkomstig zijn uit een bos.
    4. Exchange gerelateerd kenmerken (technische kenmerken niet zichtbaar in de GAL) zijn de bos bijgedragen waar `mailNickname ISNOTNULL`.
    5. Als er meerdere forests die zou voldoen aan een van deze regels, wordt klikt u vervolgens de order maken (datum/tijd) van de verbindingslijnen (forests) gebruikt om te bepalen welke bos draagt bij de kenmerken.

### <a name="contact-out-of-box-rules"></a>Neem contact op met kant-en-klare regels
Een contact-object moet voldoen aan de volgende handelingen uit die kan worden gedownload:

- De contactpersoon die moet worden ingeschakeld voor e-mail. Wordt gecontroleerd met de volgende regels:
    - `IsPresent([proxyAddresses]) = True)`. Het proxyAddresses-kenmerk moet worden ingevuld.
    - Een primaire e-mailadres vindt u in het proxyAddresses-kenmerk of het e-kenmerk. De aanwezigheid van een @ wordt gebruikt om te controleren of de inhoud is een e-mailadres. Een van deze twee regels moet worden geëvalueerd als True.
        - `(Contains([proxyAddresses], "SMTP:") > 0) && (InStr(Item([proxyAddresses], Contains([proxyAddresses], "SMTP:")), "@") > 0))`. Is er een vermelding met "SMTP:" en als er, kunt u een @ worden gevonden in de tekenreeks?
        - `(IsPresent([mail]) = True && (InStr([mail], "@") > 0)`. Is het e-kenmerk gevuld en als dat zo is, kunt een @ worden gevonden in de tekenreeks?

De volgende contactpersoon objecten zijn **niet** gesynchroniseerd met Azure AD:

- `IsPresent([isCriticalSystemObject])`. Controleer of er geen contactpersonen objecten die zijn gemarkeerd als kritiek worden gesynchroniseerd. Niet mag zijn met een standaardconfiguratie.
- `((InStr([displayName], "(MSOL)") > 0) && (CBool([msExchHideFromAddressLists])))`.
- `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`. Deze objecten goed niet in Exchange Online.
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Replicatie slachtoffer objecten worden niet worden gesynchroniseerd.

### <a name="group-out-of-box-rules"></a>Kant-en-klare regels groeperen
Een groepsobject moet voldoen aan de volgende handelingen uit die kan worden gedownload:

- Moet u minder dan 50.000 leden. Dit aantal is het aantal leden in de groep on-premises implementatie.
    - Als er meer leden voordat de synchronisatie van de eerste keer wordt gestart, wordt de groep niet gesynchroniseerd.
    - Als het aantal leden laten waarop deze is in eerste instantie gemaakt groeien, klikt u vervolgens wanneer bereikt 50.000 leden wordt stopgezet totdat het aantal lidmaatschap weer lager is dan 50.000 is gesynchroniseerd.
    - Opmerking: Het aantal 50.000 lidmaatschap wordt ook afgedwongen door Azure AD. U bent niet kan worden gesynchroniseerd groepen met meer leden, zelfs als u wijzigen of met deze regel verwijderen.
- Als de groep een **Distributiegroep is**, moet klikt u vervolgens deze eveneens e-mail is ingeschakeld. Zie [contactpersoon kant-en-klare regels](#contact-out-of-box-rules) voor deze regel wordt afgedwongen.

De volgende groepsobjecten zijn **niet** gesynchroniseerd met Azure AD:

- `IsPresent([isCriticalSystemObject])`. Zorgen dat u veel kant-en-klare objecten in Active Directory, zoals de ingebouwde beheerdersgroep, worden niet gesynchroniseerd.
- `[sAMAccountName] = "MSOL_AD_Sync_RichCoexistence"`. Oudere groep door DirSync gebruikt.
- `BitAnd([msExchRecipientTypeDetails],&amp;H40000000)`. Rolgroep.
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Replicatie slachtoffer objecten worden niet worden gesynchroniseerd.

### <a name="foreignsecurityprincipal-out-of-box-rules"></a>ForeignSecurityPrincipal kant-en-klare regels
FSP's zijn gekoppeld aan de 'een' (\*) object in de metaverse. In feite gebeurt deze koppeling alleen voor gebruikers en beveiligingsgroepen. Deze configuratie zorgt ervoor dat meerdere bos lidmaatschappen zijn opgelost en worden vertegenwoordigd correct in Azure AD.

### <a name="computer-out-of-box-rules"></a>Computer kant-en-klare regels
Een computerobject moet voldoen aan de volgende handelingen uit die kan worden gedownload:

- `userCertificate ISNOTNULL`. Alleen computers met Windows 10 vullen dit kenmerk. Alle computerobjecten met een waarde in dit kenmerk worden gesynchroniseerd.

## <a name="understanding-the-out-of-box-rules-scenario"></a>Informatie over het kant-en-klare regels-scenario
In dit voorbeeld gebruiken we een implementatie met één account bos (A), een resource bos (R) en één Azure AD-map.

![Afbeelding met scenario beschrijving](./media/active-directory-aadconnectsync-understanding-default-configuration/scenario.png)

In deze configuratie wordt ervan uitgegaan dat er is een ingeschakelde account in het bos account en een uitgeschakelde account in het bos resource aan een gekoppelde postvak.

Ons doel met de standaardconfiguratie is:

- Kenmerken die betrekking hebben op aanmeldingsproblemen wordt gesynchroniseerd vanuit de bos met de ingeschakelde account.
- Kenmerken die u in de algemene Adreslijst (algemene adreslijst vindt) wordt gesynchroniseerd vanuit de bos met het postvak. Als er geen postvak kan worden gevonden, wordt elke andere bos gebruikt.
- Als een gekoppelde postvak wordt gevonden, moet het gekoppelde account ingeschakeld voor het object worden geëxporteerd naar Azure AD worden gevonden.

### <a name="synchronization-rule-editor"></a>Synchronisatie regel Editor
De configuratie kunt weergeven en wijzigen met het hulpmiddel synchronisatie regels Editor (SRE) en een snelkoppeling naar dit vindt u in het startmenu.

![Pictogram voor synchronisatie regels-Editor](./media/active-directory-aadconnectsync-understanding-default-configuration/sre.png)

De SRE is een resource kit hulpmiddel en dit bij het synchroniseren van Azure AD Connect is geïnstalleerd. Als u wilt kunnen om hem te starten, moet u een lid van de groep ADSyncAdmins zijn. Wanneer deze wordt gestart, ziet u ongeveer zo uitziet:

![Van synchronisatieregels voor binnenkomende verbindingen](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

In dit deelvenster ziet u alle regels voor de synchronisatie hebt gemaakt voor de configuratie. Elke regel in de tabel is één regel van synchronisatie. Naar links onder regeltypen de twee verschillende typen worden weergegeven: binnenkomende en uitgaande. Inkomende en uitgaande afkomstig is van de weergave van de metaverse. U gaat voornamelijk focus op de regels voor binnenkomende verbindingen in dit overzicht. De werkelijke lijst met regels van de synchronisatie is afhankelijk van het schema gevonden in AD. Het account bos (fabrikamonline.com), hoeft niet alle services, zoals Exchange en Lync, en geen synchronisatieregels voor deze services zijn gemaakt in de bovenstaande afbeelding. Echter in de resource bos (res.fabrikamonline.com) u synchronisatie van regels voor deze services. De inhoud van de regels verschilt afhankelijk van de versie gedetecteerd. Bijvoorbeeld in een implementatie met Exchange 2013 zijn er meer kenmerk loopt geconfigureerd dan in Exchange 2010/2007.

### <a name="synchronization-rule"></a>Synchronisatie van regel
Een regel synchronisatie is een configuratieobject met een verzameling kenmerken die doorloopt als een voorwaarde is voldaan. Dit wordt ook gebruikt om te beschrijven hoe een object in de ruimte van een verbindingslijn is gerelateerd aan een object in de metaverse, bekend als **deelnemen** of **overeenkomen**. De synchronisatie-regels hebben een prioriteit van regels-waarde die aangeeft hoe ze zich verhouden tot elkaar. Een regel voor synchronisatie met een lagere numerieke waarde heeft een hogere prioriteit en in een conflict van de stroom kenmerk hogere prioriteit de conflictoplossing wins.

Als voorbeeld, kijkt u naar de synchronisatie-regel **In uit Active Directory-gebruiker AccountEnabled**. Deze regel in de SRE markeren en selecteer **bewerken**.

Aangezien deze regel een regel voor het kant-en-klare is, ontvangt u een waarschuwing bij het openen van de regel. Breng geen [wijzigingen in kant-en-klare regels](active-directory-aadconnectsync-best-practices-changing-default-configuration.md), zodat u zeker weet wat uw bedoelingen zijn. In dit geval wilt u alleen weergeven van de regel. Selecteer **Nee**.

![Synchronisatie van regels van waarschuwing](./media/active-directory-aadconnectsync-understanding-default-configuration/warningeditrule.png)

Een regel synchronisatie bevat vier configuratie gedeelten: beschrijving, een bereik filteren, Join regels en transformaties instellen.

#### <a name="description"></a>Beschrijving
De eerste sectie bevat algemene informatie, zoals een naam en beschrijving.

![Beschrijving tabblad synchroon regeleditor ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruledescription.png)

Ook vindt u informatie over welke verbonden systeem met deze regel is gerelateerd aan, welke typen in het verbonden systeem dat is van toepassing op, en het objecttype metaverse object. Het objecttype metaverse is altijd persoon ongeacht wanneer het type van het bron-object een gebruiker, iNetOrgPerson of contactpersoon is. Het objecttype metaverse moet nooit wijzigen zodat deze als een algemene is gemaakt. Het koppelingstype kan worden ingesteld op de Join, StickyJoin of inrichten. Deze instelling werkt samen met het gedeelte deelnemen aan regels en later valt.

U kunt ook zien dat deze regel synchroniseren voor Wachtwoordsynchronisatie wordt gebruikt. Als een gebruiker zich in de scope voor deze regel synchroniseren, wordt het wachtwoord vanuit on-premises gesynchroniseerd naar cloud (ervan uitgaande dat u de synchronisatiefunctie wachtwoord hebt ingeschakeld).

#### <a name="scoping-filter"></a>Een bereik filteren instellen
De sectie Filter een bereik instellen wordt gebruikt voor het configureren van wanneer u een regel synchronisatie wilt gebruiken. Aangezien de naam van de synchronisatie-regel die u bekijkt moet alleen worden toegepast voor ingeschakelde gebruikers aangeeft, het bereik is geconfigureerd, zodat de AD-kenmerk **userAccountControl** geen voor de bit 2 hebben mag instellen. Wanneer de synchronisatie-engine vindt een gebruiker in AD, wordt deze regel synchronisatie toegepast wanneer de decimale waarde 512 (ingeschakeld normale gebruiker) **userAccountControl** is ingesteld. Dit geldt niet de regel wanneer de gebruiker **userAccountControl** ingesteld op 514 (uitgeschakelde normale gebruiker heeft).

![Een bereik van tabblad in synchronisatie regeleditor instellen ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilter.png)

De scope filter bevat groepen en componenten die kunnen worden genest. Alle componenten binnen een groep moeten worden voldaan voor een regel synchronisatie om toe te passen. Wanneer meerdere groepen zijn gedefinieerd, moet klikt u vervolgens ten minste één groep worden voldaan voor de regel wilt toepassen. Dat wil zeggen een logische OR wordt geëvalueerd tussen groepen en een logische en binnen een groep wordt geëvalueerd. Een voorbeeld van deze configuratie vindt u in de uitgaande regel synchronisatie **Out naar AAD-groep deelnemen aan**. Er zijn verschillende filter groepen voor synchronisatie, bijvoorbeeld een voor beveiligingsgroepen (`securityEnabled EQUAL True`) en een voor distributiegroepen (`securityEnabled EQUAL False`).

![Een bereik van tabblad in synchronisatie regeleditor instellen ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilterout.png)

Deze regel wordt gebruikt om te bepalen welke groepen moeten worden ingericht naar Azure AD. Distributiegroepen moeten zijn ingeschakeld worden gesynchroniseerd met Azure AD e-mail, maar voor beveiligingsgroepen een e-mailbericht is niet vereist.

#### <a name="join-rules"></a>Deelnemen aan regels
De derde sectie wordt gebruikt om te configureren hoe objecten in de ruimte verbindingslijn zich verhouden tot objecten in de metaverse. De regel die u eerder hebt bekeken heeft geen eventuele configuratie voor deelnemen aan regels, zodat in plaats daarvan gaat u kijkt u naar **In uit Active Directory-gebruiker deelnemen aan**.

![Tabblad regels voor deelnemen in de regel-editor synchroniseren ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulejoinrules.png)

De inhoud van de join-regel, is afhankelijk van de overeenkomende optie is geselecteerd in de installatiewizard. Voor een binnenkomende regel, de evaluatie begint met een object in de bron verbindingslijn ruimte en elke groep in de join-regels in volgorde wordt geëvalueerd. Als een bronobject is geëvalueerd zodat deze overeenkomen met één object in de metaverse met een van de join-regels, wordt de objecten zijn gekoppeld. Als alle regels hebt is geëvalueerd en er geen overeenkomst, wordt klikt u vervolgens het koppelingstype op de pagina beschrijving gebruikt. Als deze configuratie **inrichten**is ingesteld, wordt klikt u vervolgens een nieuw object gemaakt in de doellijst, de metaverse. Als u wilt inrichten van een nieuw object naar de metaverse wordt ook wel aan **project** een object naar de metaverse.

De join-regels worden slechts één keer geëvalueerd. Als u een verbindingslijn ruimte-object en een metaverse-object zijn gekoppeld, blijven ze gekoppelde zo lang maken als het bereik van de synchronisatie-regel nog steeds tevreden is.

Bij de evaluatie van regels voor synchronisatie, moet slechts één regel voor synchronisatie met join-regels die zijn gedefinieerd in het bereik. Als meerdere regels voor synchronisatie met join regels voor één object worden gevonden, wordt een fout gegenereerd. Om die reden is het wordt aanbevolen om slechts één regel voor synchronisatie met join wanneer meerdere regels voor synchronisatie in het bereik voor een object zijn gedefinieerd. In de configuratie van de kant-en-klare voor Azure AD Connect-synchronisatie, kunnen deze regels worden gevonden door te kijken naar de naam en vinden die met het woord dat **deelnemen** aan het einde van de naam. Een regel synchronisatie zonder de join-regels gedefinieerd geldt de loopt kenmerk wanneer een andere regel van de synchronisatie van de objecten samengevoegd of deze is ingericht een nieuw object in de doellijst.

Als u de bovenstaande afbeelding bekijkt, kunt u zien dat de regel probeert deel te nemen aan **objectSID** met **msExchMasterAccountSid** (Exchange) en **msRTCSIP-OriginatorSid** (Lync), namelijk wat we van de topologie van een account-resource bos verwachten. U vinden dezelfde regel op alle forests. Aangenomen is dat elke bos een account of een resource bos kan zijn. Deze configuratie werkt ook als er accounts die in een enkel bos woont en hoeft te worden gekoppeld.

#### <a name="transformations"></a>Transformaties
De sectie transformatie Hiermee definieert u alle kenmerk overdrachten die van toepassing zijn op het object target wanneer de objecten zijn gekoppeld en het filter bereik is voldaan. Teruggaan naar de **In uit Active Directory-gebruiker AccountEnabled** regel van de synchronisatie, u de volgende transformaties vinden:

![Transformaties tabblad synchroon regeleditor ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruletransformations.png)

Als u wilt deze configuratie zetten in de context, in een Account-Resource bos-implementatie naar verwachting een ingeschakelde om rekening te vinden in de bos account en een uitgeschakelde account in de resource bos met Exchange en Lync-instellingen. De synchronisatie-regel die u bekijkt bevat de kenmerken die zijn vereist voor aanmelding en deze kenmerken moeten doorlopen van het bos wanneer er een ingeschakelde account. Alle deze kenmerk stromen zijn samenvoegen in één regel voor synchronisatie.

Een transformatie kan hebben verschillende typen: constante, Direct en expressie.

- Een constante stroom doorloopt altijd een waarde vastgelegde. Klik in het voorbeeld hierboven altijd wordt de waarde **waar** in de metaverse kenmerk met de naam **accountEnabled**.
- Een directe stroom loopt altijd de waarde van het kenmerk in de bron met het kenmerk target als-is.
- Het derde stroomtype expressie is en maakt voor meer geavanceerde configuraties.

De expressietaal is VBA (Visual Basic for Applications), zodat mensen ervaring van Microsoft Office of VBScript herkent de notatie. Kenmerken worden tussen vierkante haken bevinden, [kenmerknaam]. Kenmerknamen en functienamen zijn hoofdlettergevoelig, maar de synchronisatie regeleditor de expressies geëvalueerd en geef een waarschuwing als de expressie niet geldig is. Alle expressies worden uitgedrukt in een ononderbroken lijn met geneste functies. Als u wilt weergeven van de kracht van de taal van de configuratie, volgt de stroom voor pwdLastSet, maar met extra opmerkingen die zijn ingevoegd:

```
// If-then-else
IIF(
// (The evaluation for IIF) Is the attribute pwdLastSet present in AD?
IsPresent([pwdLastSet]),
// (The True part of IIF) If it is, then from right to left, convert the AD time format to a .Net datetime, change it to the time format used by Azure AD, and finally convert it to a string.
CStr(FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")),
// (The False part of IIF) Nothing to contribute
NULL
)
```

Zie [Lidmaatschap declaratieve inrichting expressies](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) voor meer informatie over de expressietaal voor kenmerk stromen.

### <a name="precedence"></a>De prioriteit van regels
U hebt nu enkele afzonderlijke regels van de synchronisatie hebt bekeken, maar de regels wel samenwerken in de configuratie. In sommige gevallen, is een kenmerkwaarde bijgedragen meerdere synchronisatieregels voor met het kenmerk met dezelfde target. In dit geval de prioriteit van regels voor kenmerk wordt gebruikt om te bepalen welke kenmerk WINS. Als voorbeeld, kijkt u naar het kenmerk sourceAnchor. Dit kenmerk is een belangrijk kenmerk moeten kunnen aanmelden bij Azure AD. U vindt de stroom van een kenmerk voor dit kenmerk in twee verschillende synchronisatieregels, **In uit Active Directory-gebruiker AccountEnabled** en **In uit Active Directory-gebruiker algemene**. Vanwege synchronisatie de prioriteit van regels, is het kenmerk sourceAnchor bijgedragen de bos met een ingeschakelde account eerst wanneer er verschillende objecten die zijn gekoppeld aan het object metaverse zijn. Als er geen ingeschakelde accounts, wordt de synchronisatie-engine maakt gebruik van de regel van de synchronisatie overlappende **In uit Active Directory-gebruiker algemene**. Deze configuratie zorgt ervoor dat zelfs voor accounts die worden uitgeschakeld, er nog steeds een sourceAnchor.

![Van synchronisatieregels voor binnenkomende verbindingen](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

De prioriteit van regels voor synchronisatieregels is ingesteld in groepen in de installatiewizard. Alle regels in een groep dezelfde naam hebben, maar ze zijn verbonden met verschillende verbonden mappen. De installatiewizard biedt de regel **In uit Active Directory-gebruiker deelnemen aan** hoogste prioriteit van regels en deze wordt herhaald over alle verbonden AD-mappen. Deze vervolgens verder met de volgende groepen van regels in een vooraf gedefinieerde volgorde. Binnen een groep, de regels worden toegevoegd in de volgorde waarin die de verbindingslijnen zijn toegevoegd in de wizard. Als een andere verbindingslijn is toegevoegd via de wizard, wordt de synchronisatie-regels worden nabesteld en regels voor de nieuwe verbindingslijn laatst worden ingevoegd in elke groep.

### <a name="putting-it-all-together"></a>Alles samenbrengen
We nu weten voldoende over regels voor synchronisatie kunnen voor meer informatie over de werking van de configuratie met de verschillende regels van synchronisatie. Als u een gebruiker en de kenmerken die zijn bijgedragen aan de metaverse bekijkt, worden de regels worden toegepast in de volgende volgorde:

Naam | Opmerking
:------------- | :-------------
In uit Active Directory-gebruiker Join | Regel voor het deelnemen aan de verbindingslijn ruimteobjecten met metaverse.
In uit Active Directory-gebruikersaccount is ingeschakeld | Kenmerken die zijn vereist voor aanmelden bij Azure AD en Office 365. We horen deze kenmerken van de ingeschakelde account.
In uit Active Directory-gebruiker gemene van Exchange | Kenmerken die zijn gevonden in de Global Address List. We wordt ervan uitgegaan dat de kwaliteit van de gegevens in de bos waar postvak van de gebruiker hebt gevonden meest geschikt is.
In uit Active Directory-gebruiker gemene | Kenmerken die zijn gevonden in de Global Address List. Als er een postvak zijn gevonden, kan andere gekoppelde objecten de kenmerkwaarde bijdragen.
In uit Active Directory-gebruiker Exchange | Bestaat alleen als Exchange is gevonden. Alle infrastructuur Exchange kenmerken loopt.
In uit Active Directory-gebruiker Lync | Bestaat alleen als Lync is gevonden. Alle kenmerken van de infrastructuur Lync loopt.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het configuratiemodel in het [Lidmaatschap declaratieve inrichten](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
- Meer informatie over de expressietaal in [Lidmaatschap declaratieve inrichting van expressies](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
- Doorgaan met het lezen van de werking van de configuratie kant-en-klare in [wat gebruikers en contactpersonen](active-directory-aadconnectsync-understanding-users-and-contacts.md)
- Lees hoe u een praktische wijziging met declaratieve inrichting in [hoe u een wijziging in de standaardconfiguratie](active-directory-aadconnectsync-change-the-configuration.md)aanbrengen.

**Van overzichtsonderwerpen**

- [Synchronisatie van Azure AD Connect: begrijpen en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
- [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
