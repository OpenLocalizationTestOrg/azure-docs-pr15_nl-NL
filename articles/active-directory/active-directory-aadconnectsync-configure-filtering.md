<properties
    pageTitle="Synchronisatie van Azure AD Connect: configureren filteren | Microsoft Azure"
    description="Wordt uitgelegd hoe u configureert filteren in Azure AD Connect synchroniseren."
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
    ms.date="09/13/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-configure-filtering"></a>Synchronisatie van Azure AD Connect: configureren filteren
Met filteren, kunt u bepalen welke objecten moeten worden weergegeven in Azure AD uit uw on-premises adreslijst. De standaardconfiguratie worden alle objecten in alle domeinen in de geconfigureerde forests. In het algemeen, is dit de aanbevolen configuratie. Eindgebruikers met Office 365-werkbelastingen, zoals Exchange Online en Skype voor bedrijven, profiteren van een voltooid algemene adreslijst, zodat zij kunnen e-mailbericht verzenden en iedereen bellen. Met de standaardinstellingen is geconfigureerd worden verkregen de dezelfde ervaring die ze zou met een lokale implementatie van Exchange- of Lync doen.

In sommige gevallen moet deze wijzigingen aanbrengen in de standaardconfiguratie. Hier volgen enkele voorbeelden:

- U van plan bent de [meervoudige Azure AD-directory topologie](active-directory-aadconnect-topologies.md#each-object-only-once-in-an-azure-ad-directory)gebruiken. Moet u een filter toepassen om te bepalen welk object moet worden gesynchroniseerd naar een bepaald Azure AD-map.
- Een pilot voor Azure- of Office 365 wordt uitgevoerd en u wilt dat alleen een subset van gebruikers in Azure AD. In de kleine leider is het niet belangrijk dat een volledige algemene adreslijst om te laten zien van de functionaliteit voor.
- U hebt veel serviceaccounts en andere niet-persoonlijke accounts die u niet dat in Azure AD wilt.
- Vanwege de naleving u niet elke gebruiker accounts on-premises implementatie verwijderen. U alleen uitschakelen deze. Maar in Azure AD u wilt dat alleen actieve accounts moet aanwezig zijn.

In dit artikel wordt uitgelegd hoe u de verschillende methoden die filteren configureren.

> [AZURE.IMPORTANT]Microsoft biedt geen ondersteuning wijziging of werking van de synchronisatie Azure AD Connect buiten deze acties formeel beschreven. Een van deze acties kan dit leiden tot een inconsistente of niet-ondersteunde status van Azure AD Connect-synchronisatie en daardoor Microsoft biedt geen technische ondersteuning voor deze implementaties.

## <a name="basics-and-important-notes"></a>Basisprincipes van en belangrijke notities
Klik in Azure AD Connect synchroniseren kunt u filteren op elk gewenst moment. Als u met een standaard-configuratie van adreslijstsynchronisatie begint en configureer filteren, de objecten die worden uitgefilterd niet meer gesynchroniseerd met Azure AD. Grond van deze wijziging, worden alle objecten in Azure AD die eerder zijn gesynchroniseerd, maar klik zijn gefilterd in Azure AD verwijderd.

Voordat u begint met het maken van wordt gewijzigd in filteren, Controleer of u [de geplande taak uitschakelen](#disable-scheduled-task) zodat u wijzigingen die u nog niet hebt gecontroleerd om te worden juiste niet per ongeluk exporteren.

Aangezien filteren, veel objecten tegelijkertijd verwijderen kan, die u wilt controleren of dat uw nieuwe filters juist zijn voordat u begint met het exporteren van wijzigingen naar Azure AD. Nadat u de volgende configuratiestappen uit hebt voltooid, is het raadzaam de [verificatie stappen](#apply-and-verify-changes) te volgen voordat u exporteren en wijzigingen in Azure AD aanbrengen.

Als u wilt voorkomen dat u veel objecten per ongeluk verwijderen, is de functie [niet per ongeluk verwijderen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) standaard ingeschakeld. Als u veel objecten vanwege filteren (500 al dan niet standaard) verwijdert, moet u de stappen in dit artikel toe te staan dat deze verwijdert u naar Azure AD doorlopen.

Als u een opbouwen voordat November 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)), een wijziging aanbrengt in de configuratie van het filter en u Wachtwoordsynchronisatie gebruiken, moet u een volledige synchronisatie van alle wachtwoorden activeren als u klaar bent met de configuratie. Zie [activeren voor een volledige synchronisatie van alle wachtwoorden](active-directory-aadconnectsync-implement-password-synchronization.md#trigger-a-full-sync-of-all-passwords)voor stapsgewijze instructies voor het activeren van een volledige Wachtwoordsynchronisatie. Als u op 1.0.9125 of hoger, klikt u vervolgens berekent de actie regelmatige **volledige synchronisatie** ook als u wachtwoorden synchroniseren wilt en deze extra stap niet langer vereist is.

Als **gebruikersobjecten** zijn per ongeluk hebt verwijderd in Azure AD vanwege een fout filteren, kunt u opnieuw de gebruikersobjecten maken in Azure AD door te verwijderen van uw filteren configuraties en vervolgens uw mappen opnieuw te synchroniseren. Hiermee herstelt u de gebruikers uit de Prullenbak in Azure AD. Echter niet kunt u andere objecttypen ongedaan maken. Worden bijvoorbeeld als u per ongeluk een beveiligingsgroep verwijderen en het is gebruikt voor de Toegangsbeheerlijst van een resource, de groep en de ACL's kunnen niet hersteld.

Azure AD Connect verwijdert u alleen objecten die eenmaal nieuwsbrief in het bereik. Als er objecten in Azure AD die zijn gemaakt door een andere synchronisatie-engine en deze objecten zijn niet in een bereik, toe te voegen filteren Verwijder deze. Als u met een DirSync-server begint en deze een volledig exemplaar van uw hele adreslijst hebt gemaakt in Azure AD en u een nieuwe Azure AD Connect synchronisatieserver parallel waarbij filters zijn ingeschakeld vanaf het begin is geïnstalleerd, wordt deze bijvoorbeeld niet de extra objecten die zijn gemaakt door DirSync verwijderd.

De filteren configuratie blijft behouden wanneer u installeren of naar een nieuwere versie van Azure AD Connect upgraden. Het is altijd een goede gewoonte om te bevestigen dat de configuratie is niet per ongeluk gewijzigd na een upgrade naar een nieuwere versie voordat u de eerste synchronisatie bladeren.

Als u meer dan één bos hebt, moeten de filteren configuraties die worden beschreven in dit onderwerp worden toegepast op elke bos (ervan uitgaande dat u wilt dat dezelfde configuratie voor al deze).

### <a name="disable-scheduled-task"></a>Geplande taak uitschakelen
Als u wilt de ingebouwde planner die een synchronisatie cyclus elke 30 minuten activeert uitgeschakeld, als volgt te werk:

1. Ga naar een PowerShell-prompt.
2. Uitvoeren `Set-ADSyncScheduler -SyncCycleEnabled $False` naar de planner uitgeschakeld.
3. Breng de wijzigingen, zoals beschreven in dit onderwerp.
4. Uitvoeren `Set-ADSyncScheduler -SyncCycleEnabled $True` naar de planner weer inschakelen.

**Als u een opbouwen Azure AD Connect vóór 1.1.105.0 gebruiken**  
Als u wilt uitschakelen van de taak die een synchronisatie cyclus activeert elke 3 uur als volgt te werk:

1. **Taakplanner** starten vanuit het startmenu.
2. Rechtstreeks onder **Bibliotheek voor Taakplanner**, zoek de taak met de naam **Azure AD-synchronisatie Scheduler**, klik met de rechtermuisknop en selecteer **uitschakelen**.  
![Taakplanner](./media/active-directory-aadconnectsync-configure-filtering/taskscheduler.png)  
3. U kunt nu configuratiewijzigingen en de synchronisatie-engine handmatig uitvoeren vanuit de beheerconsole van **synchronisatie-service** .

Nadat u alle wijzigingen in uw filteren hebt voltooid, vergeet niet om uit te komen terug en **Schakel** de taak opnieuw.

## <a name="filtering-options"></a>Filteropties
De volgende filteren configuratie-typen kunnen worden toegepast op het hulpprogramma Directory-synchronisatie:

- [**Groep op basis**](active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups): filteren op basis van een groep kan alleen worden geconfigureerd op de oorspronkelijke installatie met de wizard installeren. Het is niet verder bedekt in dit onderwerp.

- [**Op basis van een domein**](#domain-based-filtering): met deze optie kunt u kiezen welke domeinen die worden gesynchroniseerd naar Azure AD. Ook kunt u toevoegen aan of verwijderen van domeinen van de synchronisatie-engine configuratie als u wijzigingen in de infrastructuur van uw on-premises implementatie aanbrengt nadat u de synchronisatie van Azure AD Connect hebt geïnstalleerd.

- [**Organisatie-eenheid gebaseerde**](#organizational-unitbased-filtering): deze filteroptie kunt u die organisatie-eenheden synchroniseren met Azure AD selecteren. Deze optie is voor alle objecttypen in geselecteerde organisatie-eenheden.

- [**Kenmerk gebaseerde**](#attribute-based-filtering): deze optie kunt u filteren op basis van waarden van kenmerken op de objecten objecten. U kunt ook verschillende filters voor verschillende objecttypen hebben.

U kunt meerdere filteropties gebruiken op hetzelfde moment. Bijvoorbeeld: u kunt filteren op basis van een organisatie-eenheid alleen objecten opnemen in een organisatie-eenheid en klik in dezelfde tijd kenmerk gebaseerde filters als u wilt filteren van de objecten verder. Wanneer u meerdere filteren methoden gebruiken, gebruikt u de filters een logische en tussen de filters.

## <a name="domain-based-filtering"></a>Filteren op basis van het domein
Hier vindt u de stappen uit om de filter van uw domein te configureren. Als u hebt toegevoegd of verwijderd van domeinen in uw bos nadat u Azure AD Connect hebt geïnstalleerd, hebt u ook de filteren configuratie bij te werken.

De beste manier om te filteren op basis van een domein wijzigen is door de installatie wizard en wijzigt u het [domein en organisatie-eenheden filteren](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering)uit te voeren. De installatiewizard is alle taken beschreven in dit onderwerp automatiseren.

Volg deze stappen alleen als u om een bepaalde reden niet de installatiewizard uitvoeren.

Configuratie van filteren op basis van een domein bestaat uit de volgende stappen uit:

- [Selecteer de domeinen](#select-domains-to-be-synchronized) die moeten worden opgenomen in de synchronisatie.
- Pas het [uitvoeren van profielen](#update-run-profiles)voor elk domein toegevoegd of verwijderd.
- [Toepassen en controleer of u wijzigingen](#apply-and-verify-changes).

### <a name="select-domains-to-be-synchronized"></a>Selecteer domeinen die kan worden gedownload
**Voer de volgende stappen uit om het domein filter instelt:**

1. Meld u aan bij de server waarop Azure AD Connect synchroniseren met een account die deel uitmaakt van de beveiligingsgroep van **ADSyncAdmins** wordt uitgevoerd.
2. **Synchronisatie-Service** starten vanuit het startmenu.
3. Selecteer **verbindingslijnen** en klik in de lijst **verbindingslijnen** selecteert u de verbindingslijn met het type **Active Directory Domain Services**. Selecteer **Eigenschappen**van **Acties**.  
![Eigenschappen van een verbindingslijn](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Klik op **Active Directory-partities configureren**.
5. Selecteer in de lijst **Selecteer Active directory-partities** en hef de selectie van de domeinen naar wens. Controleer of alleen de partities die u wilt synchroniseren zijn geselecteerd.  
![Partities](./media/active-directory-aadconnectsync-configure-filtering/connectorpartitions.png)  
Als u uw on-premises hebt gewijzigd AD-infrastructuur en toegevoegde of verwijderde domeinen van de bos, klik vervolgens op de knop **vernieuwen** om een bijgewerkte lijst te verkrijgen. Wanneer u vernieuwt, wordt u gevraagd om referenties. Eventuele referenties opgeven met leestoegang naar uw lokale Active Directory. Er geen de gebruiker die is ingevuld in het dialoogvenster.  
![Vernieuwen vereist](./media/active-directory-aadconnectsync-configure-filtering/refreshneeded.png)  
6. Wanneer u klaar bent, sluit u het dialoogvenster **Eigenschappen** door te klikken op **OK**. Als u de domeinen van de bos hebt verwijderd, wordt u een bericht pop-up zeggen dat een domein is verwijderd en die configuratie opgeschoond.
7. Gaat u verder met het aanpassen van de [profielen worden uitgevoerd](#update-run-profiles).

### <a name="update-run-profiles"></a>Update uitvoeren profielen
Als u het filter van uw domein hebt bijgewerkt, moet u ook de uitvoeren profielen bijwerken.

1. Zorg dat de verbindingslijn die u in de vorige stap hebt gewijzigd is geselecteerd in de lijst **verbindingslijnen** . Selecteer **Acties** **Uitvoeren profielen configureren**.  
![Connector profielen uitvoeren](./media/active-directory-aadconnectsync-configure-filtering/connectorrunprofiles1.png)  

U moet de volgende profielen aanpassen:

- Volledige importeren
- Volledige synchronisatie
- Delta importeren
- Deltasynchronisatie
- Exporteren

Voor elk van de vijf profielen, voert u de volgende stappen uit voor elk domein **hebt toegevoegd** :

1. Selecteer het profiel dat uitvoeren en klik op **Nieuwe stap**.
2. Selecteer op de pagina **Stap configureren** in de vervolgkeuzelijst **Type** het staptype met dezelfde naam als het profiel dat u configureert. Klik vervolgens op **volgende**.  
![Connector profielen uitvoeren](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep1.png)  
3. Klik op de pagina **Configuratie Connector** in de vervolgkeuzelijst **Partition** , selecteer de naam van het domein dat u hebt toegevoegd om uw domein te filteren.  
![Connector profielen uitvoeren](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep2.png)  
4. Als u wilt sluit het dialoogvenster **Uitvoeren profiel configureren** , klik op **Voltooien**.

Voor elk van de vijf profielen, voert u de volgende stappen uit voor elk domein **verwijderd** :

1. Selecteer het profiel uitvoeren.
2. Als de **waarde** van het kenmerk **Partition** een GUID is, selecteer de stap die uitvoeren en klik op **Stap verwijderen**.  
![Connector profielen uitvoeren](./media/active-directory-aadconnectsync-configure-filtering/runprofilesdeletestep.png)  

Het resultaat moet dat elk domein dat u wilt synchroniseren, moet staan vermeld als een stap in elk profiel uitvoeren.

Als u wilt sluit het dialoogvenster **Uitvoeren profielen configureren** , klik op **OK**.

- Om te voltooien van de configuratie, [toepassen en controleer of u wijzigingen](#apply-and-verify-changes).

## <a name="organizational-unitbased-filtering"></a>Organisatie-eenheid gebaseerde filteren
De beste manier om te filteren op basis van een organisatie-eenheid wijzigen is door de installatie wizard en wijzigt u het [domein en organisatie-eenheden filteren](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering)uit te voeren. De installatiewizard is alle taken beschreven in dit onderwerp automatiseren.

Volg deze stappen alleen als u om een bepaalde reden niet de installatiewizard uitvoeren.

**Als u wilt configureren organisatie-eenheid – gebaseerde filteren, voer de volgende stappen uit:**

1. Meld u aan bij de server waarop Azure AD Connect synchroniseren met een account die deel uitmaakt van de beveiligingsgroep van **ADSyncAdmins** wordt uitgevoerd.
2. **Synchronisatie-Service** starten vanuit het startmenu.
3. Selecteer **verbindingslijnen** en klik in de lijst **verbindingslijnen** selecteert u de verbindingslijn met het type **Active Directory Domain Services**. Selecteer **Eigenschappen**van **Acties**.  
![Eigenschappen van een verbindingslijn](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. **Active Directory-partities configureren**op, selecteer het domein dat u wilt configureren en klik vervolgens op **Containers**.
5. Wanneer u wordt gevraagd, eventuele referenties opgeven met leestoegang naar uw lokale Active Directory. Er geen de gebruiker die is ingevuld in het dialoogvenster.
6. Schakel in het dialoogvenster **Selecteer Containers** de organisatie-eenheden die u niet wilt synchroniseren met de cloud-map en klik vervolgens op **OK**.  
![ORGANISATIE-EENHEID](./media/active-directory-aadconnectsync-configure-filtering/ou.png)  
  - De container **Computers** moet zijn ingeschakeld voor uw computers met Windows 10 om te worden gesynchroniseerd met Azure AD. Als uw domein deel uitmaakt van computers zich bevinden in andere organisatie-eenheden, zorgt u ervoor die zijn geselecteerd.
  - De container **ForeignSecurityPrincipals** moet zijn geselecteerd als er meerdere forests met vertrouwensrelaties. Deze container kunt cross-bos beveiligingsgroep worden omgezet.
  - De **RegisteredDevices** organisatie-eenheid moet worden geselecteerd als u de functie van de write-backs apparaat hebt ingeschakeld. Als u een andere write-backs-functie, zoals groep write-backs, controleert u of dat deze locaties zijn geselecteerd.
  - Selecteer een andere organisatie-eenheid waar gebruikers, iNetOrgPersons, groepen, contactpersonen en Computers zich bevinden. In de afbeelding, worden alle deze zich in de OU ManagedObjects.
7. Wanneer u klaar bent, sluit u het dialoogvenster **Eigenschappen** door te klikken op **OK**.
8. Om te voltooien van de configuratie, [toepassen en controleer of u wijzigingen](#apply-and-verify-changes).

## <a name="attribute-based-filtering"></a>Kenmerk gebaseerde filteren
Zorg ervoor dat u op de November 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)) of later maken voor de volgende stappen uit om te werken.

Filteren op basis van kenmerk is de eenvoudigste manier om te filterobjecten. U kunt de kracht van [declaratieve inrichting](active-directory-aadconnectsync-understanding-declarative-provisioning.md) vrijwel elke aspecten van bepalen wanneer een object moet worden gesynchroniseerd naar Azure AD.

U kunt filters toepassen op de [binnenkomende](#inbound-filtering) van Active Directory naar de metaverse en [uitgaande](#outbound-filtering) uit de metaverse naar Azure AD. Het wordt aanbevolen om toe te passen filteren op inkomende aangezien die het gemakkelijkst onderhouden. Uitgaande filtering moet alleen worden gebruikt als dit vereist is voor deelname aan objecten van meer dan één bos voordat de evaluatie kan plaatsvinden.

### <a name="inbound-filtering"></a>Inkomende filteren
Binnenkomende op basis van filteren maakt gebruik van de standaardconfiguratie objecten gaat u naar Azure AD moet waar de metaverse kenmerk cloudFiltered niet is ingesteld op een waarde die kan worden gedownload. Als de waarde van dit kenmerk is ingesteld op **waar**, klikt u vervolgens het object niet gesynchroniseerd. Deze mag niet worden ingesteld op **Onwaar** inherent aan het ontwerp. Om ervoor te zorgen dat andere regels hebt de mogelijkheid om een waarde mee te, dit kenmerk is alleen moet zijn de waarden **waar** of **NULL** (afwezig).

In de binnenkomende filteren gebruikt u de kracht van **bereik** om te bepalen welke objecten moeten of niet moeten worden gesynchroniseerd. Dit is waar u pas aanpassen aan de behoeften van uw eigen organisatie. De module bereik heeft **groeperen** en **component** om te bepalen of een regel voor synchronisatie in omvang moet. Een **groep** bevat een of meer **component**. Er is een logische en tussen meerdere componenten en een logische of tussen meerdere groepen.

Laat ons naar een voorbeeld kijken:  
![Bereik](./media/active-directory-aadconnectsync-configure-filtering/scope.png) moet dit worden gelezen als **(afdeling = IT) of (afdeling = verkoop-en c = US)**.

In de voorbeelden en de onderstaande stappen, u het gebruikersobject gebruiken als voorbeeld, maar u kunt dit gebruiken voor alle objecttypen.

In de onderstaande voorbeelden wordt de waarde van prioriteit van regels met 500 beginnen. Deze waarde zorgt ervoor dat deze regels worden geëvalueerd na de kant-en-klare regels (lagere prioriteit, hogere numerieke waarde).

#### <a name="negative-filtering-do-not-sync-these"></a>Negatieve filteren, "niet synchroniseer deze"
Klik in het volgende voorbeeld wordt u uitfilteren (niet worden gesynchroniseerd) alle gebruikers waarvoor **extensionAttribute15** de waarde **NoSync**hebben.

1. Meld u aan bij de server waarop Azure AD Connect synchroniseren met een account die deel uitmaakt van de beveiligingsgroep van **ADSyncAdmins** wordt uitgevoerd.
2. Start **Synchronisatie regeleditor** in het startmenu.
3. Controleer of **dat inkomend** is geselecteerd en klik op **Nieuwe regel toevoegen**.
4. Geef de regel een beschrijvende naam, zoals '*In uit Active Directory-gebruiker DoNotSyncFilter*". Selecteer de juiste bos, **gebruiker** als het **objecttype CS**en **persoon** als het **objecttype MV**. Als het **Type koppeling**, selecteert u **deelnemen aan** en typ een waarde die is momenteel niet worden gebruikt door een andere regel voor synchronisatie (bijvoorbeeld 500) in de prioriteit van regels en klik vervolgens op **volgende**.  
![Binnenkomende 1 Beschrijving](./media/active-directory-aadconnectsync-configure-filtering/inbound1.png)  
5. In **Scoping filteren**, klikt u op **Groep toevoegen**, klikt u op **Component toevoegen**en selecteer **ExtensionAttribute15**in kenmerk. Zorg ervoor dat de Operator is ingesteld op **gelijk** en typ de waarde **NoSync** in het vak waarde. Klik op **volgende**.  
![Inkomende 2 bereik](./media/active-directory-aadconnectsync-configure-filtering/inbound2.png)  
6. Laat de regels voor het **deelnemen aan** leeg en klik vervolgens op **volgende**.
7. Klik op **Transformatie toevoegen**, selecteer de **FlowType** naar **constante**het kenmerk Target **cloudFiltered** en typ in het vak bron **waar**. Klik op **toevoegen** om op te slaan van de regel.  
![Inkomende 3-transformatie](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)
8. Om te voltooien van de configuratie, [toepassen en controleer of u wijzigingen](#apply-and-verify-changes).

#### <a name="positive-filtering-only-sync-these"></a>Positieve filteren, "alleen synchroniseren deze"
Uitdrukken positieve filteren kan lastig zijn meer sinds u ook rekening houden met objecten die niet worden gesynchroniseerd moet, zoals vergaderruimten duidelijk zijn.

De positieve filteroptie vereist twee regels van synchronisatie. Een (of meer) met het juiste bereik van objecten die u wilt synchroniseren en een tweede regel overlappende synchroniseren dat filter uit alle objecten die nog niet zijn geïdentificeerd als een object die moet worden gesynchroniseerd.

In het volgende voorbeeld, moet u alleen gebruikersobjecten waarvan het kenmerk afdeling de waarde **Sales bevat**synchroniseren.

1. Meld u aan bij de server waarop Azure AD Connect synchroniseren met een account die deel uitmaakt van de beveiligingsgroep van **ADSyncAdmins** wordt uitgevoerd.
2. Start **Synchronisatie regeleditor** in het startmenu.
3. Controleer of **dat inkomend** is geselecteerd en klik op **Nieuwe regel toevoegen**.
4. Geef een beschrijvende naam, bijvoorbeeld '*In uit Active Directory-gebruiker verkoop synchroniseren*' voor de regel. Selecteer de juiste bos, **gebruiker** als het **objecttype CS**en **persoon** als het **objecttype MV**. Als het **Type koppeling**, selecteert u **deelnemen aan** en typ een waarde die is momenteel niet worden gebruikt door een andere regel voor synchronisatie (bijvoorbeeld 501) in de prioriteit van regels en klik vervolgens op **volgende**.  
![Inkomende 4 beschrijving](./media/active-directory-aadconnectsync-configure-filtering/inbound4.png)  
5. In **Scoping filteren**, klikt u op **Groep toevoegen**op **Component toevoegen**en selecteer in kenmerk **afdeling**. Zorg ervoor dat de Operator is ingesteld op **gelijk** en typ de waarde **verkoop** in het vak waarde. Klik op **volgende**.  
![Inkomende 5 bereik](./media/active-directory-aadconnectsync-configure-filtering/inbound5.png)  
6. Laat de regels voor het **deelnemen aan** leeg en klik vervolgens op **volgende**.
7. Klik op **Transformatie toevoegen**, selecteer de **FlowType** naar **constante**het kenmerk Target **cloudFiltered** en typ in het vak bron tekst **Onwaar**. Klik op **toevoegen** om op te slaan van de regel.  
![Inkomende 6-transformatie](./media/active-directory-aadconnectsync-configure-filtering/inbound6.png)  
Dit is een speciale geval waar u cloudFiltered expliciet ingesteld op onwaar.

    We hebben nu de overlappende synchronisatie-regel maken.

8. Geef de regel een beschrijvende naam, zoals '*In uit Active Directory-gebruiker variabel-alle filter*". Selecteer de juiste bos, **gebruiker** als het **objecttype CS**en **persoon** als het **objecttype MV**. Als het **Type koppeling**, selecteert u **deelnemen aan** en typ een waarde die is momenteel niet worden gebruikt door een andere regel voor synchronisatie (bijvoorbeeld 600) in de prioriteit van regels. U hebt geselecteerd van een prioriteit van regels voor waarde hoger (lagere prioriteit) dan de vorige regel van de synchronisatie, maar ook enkele ruimte links, zodat we meer filteren synchronisatie regels later toevoegen kunt wanneer u wilt beginnen met het synchroniseren van extra afdelingen. Klik op **volgende**.  
![Inkomende 7 beschrijving](./media/active-directory-aadconnectsync-configure-filtering/inbound7.png)  
9. Laat **Scoping filter** leeg en klik op **volgende**. Een leeg filter geeft aan dat de regel moet worden toegepast op alle objecten.
10. Laat de regels voor het **deelnemen aan** leeg en klik vervolgens op **volgende**.
11. Klik op **Transformatie toevoegen**, selecteer de **FlowType** naar **constante**het kenmerk Target **cloudFiltered** en typ in het vak bron **waar**. Klik op **toevoegen** om op te slaan van de regel.  
![Inkomende 3-transformatie](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)  
12. Om te voltooien van de configuratie, [toepassen en controleer of u wijzigingen](#apply-and-verify-changes).

Als u wilt, kunt u meer regels van de eerste type waarin u meerdere objecten in onze synchronisatie opnemen maken.

### <a name="outbound-filtering"></a>Uitgaande filteren
In sommige gevallen is het nodig moet u doen met het filter alleen nadat de objecten in de metaverse hebt toegevoegd. Bijvoorbeeld mogelijk verplicht voor het kenmerk e-mail bekijken van de resource bos en het userPrincipalName-kenmerk van het account bos om te bepalen als een object moet worden gesynchroniseerd. In dat geval maakt u de filteren op de uitgaande regel.

In dit voorbeeld die u wijzigt de filteren zodat alleen gebruikers die waar e-mail- en userPrincipalName eindigt op @contoso.com worden gesynchroniseerd:

1. Meld u aan bij de server waarop Azure AD Connect synchroniseren met een account die deel uitmaakt van de beveiligingsgroep van **ADSyncAdmins** wordt uitgevoerd.
2. Start **Synchronisatie regeleditor** in het startmenu.
3. Klik onder **Type regels**, klikt u op **Uitgaand**.
4. Zoek de regel met de naam **Out naar AAD-gebruiker deelnemen aan SOAInAD**. Klik op **bewerken**.
5. Klik in het pop-upvenster beantwoord **Ja** als een kopie van de regel wilt maken.
6. Klik op de pagina **Beschrijving** wijzigen de prioriteit van regels op een niet-gebruikte waarde, bijvoorbeeld 50.
7. Klik op **Scoping filter** in het linkernavigatievenster. Klik op **component toevoegen**, in kenmerk select **mail**, in de Operator select **ENDSWITH**en in waardetype **@contoso.com**. Klik op **component toevoegen**, in kenmerk select **userPrincipalName**, Operator select **ENDSWITH**en in waardetype **@contoso.com**.
8. Klik op **Opslaan**.
9. Om te voltooien van de configuratie, [toepassen en controleer of u wijzigingen](#apply-and-verify-changes).

## <a name="apply-and-verify-changes"></a>Toepassen en wijzigingen controleren
Nadat u de configuratiewijzigingen hebt aangebracht, moeten deze worden toegepast op de objecten die al in het systeem. Dat objecten momenteel niet in de synchronisatie-engine moeten worden verwerkt en wordt de synchronisatie-engine moet lezen van het bronsysteem opnieuw ter controle van de inhoud kan ook worden.

Als u een **domein** of **organisatie-eenheid** filteren met de configuratie gewijzigd, moet u doen **volledige importeren** , gevolgd door **Deltasynchronisatie**.

Als u met behulp van **kenmerk** filteren configuratie gewijzigd, moet u doen **volledige synchronisatie**.

De volgende stappen uitvoeren:

1. **Synchronisatie-Service** starten vanuit het startmenu.
2. Selecteer **verbindingslijnen** en klik in de lijst **verbindingslijnen** selecteert u de verbindingslijn waarin u een configuratie wijzigen eerder hebt aangebracht. Selecteer **Acties** **uitvoeren**.  
![Connector uitvoeren](./media/active-directory-aadconnectsync-configure-filtering/connectorrun.png)  
3. Selecteer in de die wordt **uitgevoerd profielen**, de bewerking die worden genoemd in de vorige sectie. Als u twee acties uitvoeren moet, voert u de tweede na de eerste fase (de kolom **status** is **niet-actief** voor de geselecteerde verbindingslijn).

Na de synchronisatie, worden alle wijzigingen gefaseerde om te worden geëxporteerd. Voordat u daadwerkelijk de wijzigingen in Azure AD aanbrengen, die u wilt controleren of alle deze wijzigingen juist zijn.

1. Start een cmd-prompt en Ga naar`%Program Files%\Microsoft Azure AD Sync\bin`
2. Uitvoeren:`csexport "Name of Connector" %temp%\export.xml /f:x`  
De naam van de verbindingslijn vindt u in de synchronisatieservice. Een naam die vergelijkbaar is met "contoso.com – AAD" voor Azure AD.
3. Uitvoeren:`CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`
4. U hebt nu een bestand in % temp % benoemde export.csv die kan worden onderzocht in Microsoft Excel. Dit bestand bevat alle wijzigingen die zullen worden geëxporteerd.
5. Noodzakelijke wijzigingen aanbrengen in de gegevens of de configuratie en uitvoeren van deze stappen nogmaals (importeren, synchroniseren en verifiëren) totdat de wijzigingen die moeten worden geëxporteerd naar verwachting.

Wanneer u tevreden bent, exporteert u de wijzigingen naar Azure AD.

1. Selecteer **verbindingslijnen** en klik in de lijst **verbindingslijnen** selecteert u de verbindingslijn van Azure AD. Selecteer **Acties** **uitvoeren**.
2. Selecteer **exporteren**in het venster het **uitvoeren van profielen**.
3. Als uw configuratiewijzigingen veel objecten verwijderen, klikt u vervolgens ziet u een fout op de export wanneer het getal groter dan de drempelwaarde voor geconfigureerde (al dan niet standaard 500 is). Als u deze fout wordt weergegeven, moet u tijdelijk uitschakelen van de functie [niet per ongeluk verwijderen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md).

Nu is het tijd om de planner weer inschakelen.

1. **Taakplanner** starten vanuit het startmenu.
2. Rechtstreeks onder **Bibliotheek voor Taakplanner**, zoek de taak met de naam **Azure AD-synchronisatie Scheduler**, klik met de rechtermuisknop en selecteer **inschakelen**.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de configuratie [Azure AD Connect synchroniseren](active-directory-aadconnectsync-whatis.md) .

Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).
