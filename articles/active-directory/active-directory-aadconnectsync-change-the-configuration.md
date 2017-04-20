<properties
    pageTitle="Synchronisatie van Azure AD Connect: hoe u een wijziging in de standaardconfiguratie | Microsoft Azure"
    description="Begeleidt u bij het maken van een wijziging in de configuratie in Azure AD Connect synchroniseren."
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


# <a name="azure-ad-connect-sync-how-to-make-a-change-to-the-default-configuration"></a>Synchronisatie van Azure AD Connect: hoe u een wijziging in de standaardconfiguratie
Het doel van dit onderwerp wordt u stapsgewijs bij het aanbrengen van wijzigingen in de standaardconfiguratie in Azure AD Connect synchroniseren. Deze stapsgewijze instructies voor enkele veelvoorkomende scenario's. Met deze kennis moet u mogelijk enkele eenvoudige wijzigingen aanbrengen in uw eigen configuratie op basis van uw eigen bedrijfsregels.

## <a name="synchronization-rules-editor"></a>Synchronisatie regeleditor
De synchronisatie regeleditor wordt gebruikt om te zien en wijzigen van de standaardconfiguratie. U vindt deze in het Menu Start in de groep **Azure AD Connect** .  
![Startmenu met synchronisatie regel Editor](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

Wanneer u deze opent, ziet u de standaard kant-en-klare regels.

![Synchronisatie regel Editor](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>Navigeren in de editor
De vervolgkeuzelijsten boven aan de editor kunnen u snel zoeken naar een bepaalde regel zijn aangetroffen. Als u zien van de regels waarin het proxyAddresses kenmerk is opgenomen wilt, zou u bijvoorbeeld de vervolgkeuzelijsten wijzigen als volgt uit:  
![SRE filteren](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png)  
Als u opnieuw filteren en een vers configuratie laden, drukt u op het toetsenbord op **F5** .

Naar het juiste begin hebt u een knop **nieuwe regel toevoegen**. Deze knop wordt gebruikt om uw eigen aangepaste regel maken.

Onderaan, kunt u knoppen voor het optreden hebt op een regel voor het geselecteerde synchroniseren. **Bewerken** en **verwijderen** doen wat u verwacht. **Exporteren** , genereert een PowerShell-script voor het opnieuw te maken van de synchronisatie-regel. Deze procedure kunt u een regel voor het synchroniseren van de ene server naar de andere verplaatsen.

## <a name="create-your-first-custom-rule"></a>Uw eerste aangepaste regel maken
De meest voorkomende wijziging is wijzigingen in de loopt kenmerk. De gegevens in uw adreslijst bron mogelijk niet zoals Azure AD in. In het voorbeeld in deze sectie die u wilt controleren of dat de opgegeven naam van een gebruiker is altijd **juiste**hoofdlettergebruik.

### <a name="disable-the-scheduler"></a>De planner uitgeschakeld
De [scheduler](active-directory-aadconnectsync-feature-scheduler.md) wordt elke 30 minuten standaard uitgevoerd. U wilt controleren of dat deze niet start terwijl u wijzigingen aanbrengen wilt en problemen met uw nieuwe regels. Als u wilt tijdelijk de planner uitgeschakeld, PowerShell starten en uitvoeren`Set-ADSyncScheduler -SyncCycleEnabled $false`

![De planner uitgeschakeld](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>Maak de regel

1. Klik op **nieuwe regel toevoegen**.
2. Klik op de pagina **Beschrijving** Typ het volgende:  
![Inkomende regel filteren](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)  
    - Naam: Geef een beschrijvende naam voor de regel.
    - Beschrijving: Sommige toelichting zodat iemand anders wat de regel begrijpen kan voor is.
    - Systeem verbonden: het systeem het object vindt u in. In dit geval selecteert u de Active Directory-Connector.
    - Verbonden systeem/Metaverse objecttype: Selecteer respectievelijk **gebruiker** en een **persoon** .
    - Koppelingstype: Verander deze waarde op te **nemen aan**.
    - De prioriteit van regels: Geef een waarde die uniek is in het systeem. Een lagere numerieke waarde wordt aangegeven hogere prioriteit.
    - Tag: Laat leeg. Alleen kant-en-klare regels van Microsoft, moeten dit selectievakje gevuld met een waarde hebben.
3. Voer op de pagina **Scoping filter** **givenName ISNOTNULL**.  
![Inkomende regel bereik filteren instellen](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png)  
In deze sectie wordt gebruikt om te bepalen welke objecten dat de regel moet toepassen op. Als leeg laat, wordt de regel wilt toepassen op alle gebruikersobjecten. Maar dat zou bevatten, vergaderruimten, serviceaccounts en andere niet-personen gebruikersobjecten.
4. Klik op het **deelnemen aan regels**laat deze leeg.
5. Klik op de pagina **transformaties** omzetten in de FlowType **expressie**. Selecteer het kenmerk Target **givenName**en voer in de bron `PCase([givenName])`.
![Binnenkomende regel transformaties](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png)  
De synchronisatie-engine is hoofdlettergevoelig zowel op de functienaam van de en de naam van het kenmerk. Als u iets mis is typt, ziet u een waarschuwing wanneer u de regel toevoegen. De editor kunt u opslaan en doorgaan, zodat u moeten zou opnieuw opent de regel en corrigeren van de regel.
6. Klik op **toevoegen** om op te slaan van de regel.

Uw nieuwe aangepaste regel moet zijn zichtbaar met de andere regels van de synchronisatie in het systeem.

### <a name="verify-the-change"></a>Controleer de wijziging
Met deze nieuwe wijziging die u wilt Zorg ervoor dat deze werkt zoals verwacht en is niet eventuele fouten genereren. Afhankelijk van het aantal objecten die u hebt, zijn er twee verschillende manieren Voer deze stap.

1. Een volledige synchronisatie worden uitgevoerd op alle objecten
2. Een voorbeeld weergeven en de volledige synchronisatie worden uitgevoerd op één object

**Synchronisatie-Service** starten vanuit het startmenu. De stappen in deze sectie zijn alle in dit hulpprogramma.

1. **Volledige synchroniseren op alle objecten**  
Selecteer **verbindingslijnen** aan de bovenkant. De verbindingslijn die u een wijziging hebt aangebracht in de vorige sectie, in dit geval de Active Directory Domain Services, identificeren en selecteer deze. Selecteer **uitvoeren** van acties en selecteert u **Volledige synchronisatie** en kies **OK**.
![Volledige synchroniseren](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png)  
De objecten zijn nu bijgewerkt in de metaverse. Nu wilt u kijkt u naar het object in de metaverse.

2. **Preview en volledige synchroniseren op één object**  
Selecteer **verbindingslijnen** aan de bovenkant. De verbindingslijn die u een wijziging hebt aangebracht in de vorige sectie, in dit geval de Active Directory Domain Services, identificeren en selecteer deze. Selecteer **Zoeken verbindingslijn ruimte**. Gebruik scope zoeken naar een object dat u wilt gebruiken om te testen van de wijziging. Selecteer het object en klik op **voorbeeld**. Selecteer in het scherm Nieuw **Doorvoeren Preview**.
![Voorbeeld doorvoeren](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png)  
De wijziging is nu vastgelegd voor de metaverse.

**Bekijk het object in de metaverse**  
Nu wilt u kiest u een paar steekproef objecten om ervoor te zorgen dat de waarde wordt verwacht en dat de regel toegepast. Selecteer **Metaverse zoeken** vanaf de bovenkant. Voeg filter die u nodig hebt om de relevante objecten te zoeken. Open een object in het zoekresultaat. Bekijk de kenmerkwaarden en ook controleren of in de **Regels van de synchronisatie** -kolom die de regel wordt toegepast als verwacht.  
![Metaverse zoeken](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)  
### <a name="enable-the-scheduler"></a>De planner inschakelen
Als alles zoals verwacht, kunt u de planner opnieuw inschakelen. Uitvoeren vanaf PowerShell, `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>De wijzigingen van andere algemene kenmerken-stroom
De vorige sectie wordt beschreven hoe u de wijzigingen aanbrengen in een stroom kenmerk. In dit gedeelte worden enkele aanvullende voorbeelden worden gegeven. De stappen voor het maken van de synchronisatie-regel afgekorte is, maar u kunt de volledige stappen vinden in de vorige sectie.

### <a name="use-another-attribute-than-the-default"></a>Gebruik een ander kenmerk dan het standaardgeluid
Bij Fabrikam is er een bos waar het lokale alfabet wordt gebruikt voor voornaam, achternaam en naam weer te geven. De tekenweergave Latijnse van de volgende kenmerken vindt u in de kenmerken van de extensie. Bij het maken van de algemene adreslijst in Azure AD en Office 365, de organisatie wil deze kenmerken moet worden gebruikt.

Met een standaardconfiguratie, is een object uit de lokale bos ziet er zo uit:  
![Kenmerk stroom 1](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

U kunt een regel maken met andere stromen kenmerk, door het volgende te doen:

- Start **Synchronisatie regel Editor** in het startmenu.
- Met **Inkomend** nog steeds naar links is geselecteerd, klikt u op de knop **nieuwe regel toevoegen**.
- Geef een naam en beschrijving voor de regel. Selecteer de lokale Active Directory en de relevante objecttypen.  Selecteer **deelnemen**in **Koppelingstype**. Voor de prioriteit van regels, kiest u een getal dat niet wordt gebruikt door een andere regel. De regels kant-en-klare begint met 100, zodat de waarde 50 kan worden gebruikt in dit voorbeeld.
![Kenmerk stroom 2](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
- Laat bereik leeg (dat wil zeggen, moet zijn van toepassing op alle gebruikersobjecten in de bos).
- Laat join regels leeg (dat wil zeggen, laat de kant-en-klare regel verwerken van alle joins).
- Maak de volgende loopt in transformaties:  
![Kenmerk stroom 3](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
- Klik op **toevoegen** om op te slaan van de regel.
- Ga naar **Service-beheer van synchronisatie**. Klik op **verbindingslijnen**, selecteert u de verbindingslijn die waar we de regel hebt toegevoegd. Selecteer **uitvoeren**en **volledige synchronisatie**. Een volledige synchronisatie wordt herberekend alle objecten met behulp van de huidige regels.

Dit is het resultaat voor hetzelfde object met deze aangepaste regel:  
![Kenmerk stroom 4](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Lengte van kenmerken
Tekenreekskenmerken zijn standaard ingesteld moeten worden geïndexeerd en de maximale duur is 448 tekens. Als u met tekenreekskenmerken die meer bevat werkt mogelijk, klikt u vervolgens u Zorg ervoor dat de volgende handelingen uit in de stroom kenmerk:  
`attributeName` <- `Left([attributeName],448)`

### <a name="changing-the-userprincipalsuffix"></a>De userPrincipalSuffix wijzigen
Het kenmerk userPrincipalName in Active Directory is niet altijd bekend onder de gebruikers en mogelijk niet geschikt als de aanmeldings-ID. De Azure AD Connect wizard van de synchronisatie-installatie kunt kiezen van een ander kenmerk, bijvoorbeeld e-mail. Maar in sommige gevallen het kenmerk moet worden berekend. Het bedrijf Contoso bevat bijvoorbeeld twee Azure AD-mappen, één voor productie en één voor het testen. De gebruikers in hun test-tenant naar een ander achtervoegsel gebruiken in de aanmeldings-ID.  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`

In deze expressie, duren, alles links van de eerste @-sign (Word) en tekst.samenvoegen met een vaste tekenreeks.

### <a name="convert-a-multi-value-to-a-single-value"></a>Een Multi-Value converteren naar een enkele waarde
Bepaalde kenmerken in Active Directory zijn met meerdere waarden in het schema, zelfs als ze één waarde in Active Directory-gebruikers en Computers eruitzien. Een voorbeeld is het beschrijvingskenmerk.  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`

In deze expressie geval het kenmerk een waarde heeft, we het eerste item (Item) maken in het kenmerk, voorloopspaties en volgspaties (knippen) verwijderen en vervolgens behouden de eerste 448 tekens (links) in de tekenreeks.

### <a name="do-not-flow-an-attribute"></a>Een kenmerk niet flow
Achtergrondinformatie over het scenario voor deze sectie, raadpleegt u [het proces voor het stroom van kenmerk beheren](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process).

Er zijn twee manieren om te niet flow een kenmerk. De eerste is beschikbaar in de installatiewizard en kunt u [geselecteerde kenmerken](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering)verwijderen. Deze optie werkt als u het kenmerk voordat u nooit hebt gesynchroniseerd. Echter als u dit kenmerk synchroniseren en deze later te verwijderen met deze functie hebt, klikt u vervolgens de synchronisatie-engine kleurovergangsbeëindigingen beheer van het kenmerk en de bestaande waarden blijven Azure AD.

Als u wilt verwijderen van de waarde van een kenmerk en zorg ervoor dat deze wordt niet in de toekomst uitgebreid, moet u in plaats daarvan een aangepaste regel maken.

Bij Fabrikam, hebben we gerealiseerd dat enkele van de kenmerken die we in de cloud synchroniseren niet er worden moeten. We horen om ervoor te zorgen dat deze kenmerken worden verwijderd uit Azure AD.  
![Ongeldige extensie kenmerken](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

- Een nieuwe regel voor binnenkomende synchronisatie maken en de beschrijving van de vullen ![beschrijvingen](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png)
- Kenmerk stromen van het type **expressie** en aan de bron **AuthoritativeNull**maken. De letterlijke **AuthoritativeNull** wordt aangegeven dat de waarde leeg in de MV zijn moet zelfs als een lagere prioriteit van regels voor synchronisatie regel probeert aan te vullen de waarde.
![Transformatie voor extensie kenmerken](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
- De synchronisatie-regel opslaan. **Synchronisatie-Service**starten, de verbindingslijn zoeken, selecteert u **uitvoeren**en **Volledige synchronisatie**. Deze stap wordt herberekend alle kenmerk stromen.
- Controleer of de beoogde wijzigingen over die de verbindingslijn ruimte zoeken worden geëxporteerd.
![Gefaseerde verwijderen](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het configuratiemodel in het [Lidmaatschap declaratieve inrichten](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
- Meer informatie over de expressietaal in [Lidmaatschap declaratieve inrichting van expressies](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Van overzichtsonderwerpen**

- [Synchronisatie van Azure AD Connect: begrijpen en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
- [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
