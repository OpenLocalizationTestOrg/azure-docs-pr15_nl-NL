<properties
    pageTitle="Beveiliging van Azure Active Directory-identiteit | Microsoft Azure"
    description="Informatie over hoe de beveiliging van de Azure AD identiteit kan u de mogelijkheid van een hacker misbruik van een beschadigde identiteit of het apparaat en te beveiligen van een identiteit of een apparaat die eerder is verdachte of bekende gevaar lopen beperken."
    services="active-directory"
    keywords="beveiliging in de Azure active directory-identiteit, cloud-app discovery,-toepassingen, beveiliging, risico, risiconiveau, beveiligingsprobleem, beveiligingsbeleid beheren"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection"></a>Beveiliging van Azure Active Directory-identiteit 

Azure Active Directory identiteitsbeveiliging is een functie van de Azure AD Premium P2 editie biedt u een samengevoegde weergave in de risico's en potentiële problemen invloed zijn op de identiteit van uw organisatie. Microsoft heeft cloudgebaseerde identiteiten voor zijn beveiligen via een tien jaar en met Azure AD identiteit beveiliging, Microsoft is deze dezelfde beveiliging-systemen beschikbaar maken voor zakelijke klanten. Identiteitsbeveiliging maakt gebruik van de bestaande Azure AD afwijking detectiemogelijkheden (beschikbaar via Azure AD afwijkende activiteitsrapporten), en maakt u kennis met nieuwe risico gebeurtenistypen die afwijkingen in realtime kunnen detecteren.



##<a name="getting-started"></a>Aan de slag

De meeste beveiliging inbreuk op plaatsvinden wanneer onbevoegden toegang krijgen tot een omgeving met de identiteit van een gebruiker worden gestolen door. Hackers geworden steeds effectieve aan gebruikmaken van derden inbreuk op, en het gebruik van geavanceerde phishing-aanvallen. Zodra onbevoegden toegang tot zelfs een lage bevoegdheden gebruikersaccount krijgt, is relatief eenvoudig voor hen toegang te krijgen tot belangrijke bedrijfsresources tot en met zijkanten verkeer. Daarom moet u alle identiteiten beveiligen en, wanneer een identiteit is is gehackt, proactief te voorkomen dat de beschadigde identiteit geschonden. 

Beschadigde identiteiten ontdekken is geen eenvoudige taak. Gelukkig identiteitsbeveiliging kunnen helpen: identiteitsbeveiliging maakt gebruik van geavanceerde machine learning algoritmen en heuristiek detecteren afwijkingen en risico gebeurtenissen die aangeven mogelijk dat een identiteit meer veilig is.
 
Met deze gegevens, identiteitsbeveiliging genereert rapporten en waarschuwingen waarmee u kunt deze gebeurtenissen risico onderzoeken en voert u juiste remediation of risicobeperking actie.
 
Maar Azure Active Directory identiteitsbeveiliging is meer dan een hulpmiddel voor controle en rapporten. Op basis van risico's, berekent identiteitsbeveiliging een niveau van de gebruiker risico voor elke gebruiker, zodat u kunt de risico's gebaseerde beleidsregels voor als u wilt beveiligen automatisch de identiteit van uw organisatie configureren.  Dit beleid op basis van het risico, naast andere voorwaardelijke toegang tot besturingselementen voor verstrekt door Azure Active Directory en EMS, kunnen automatisch blokkeren of bieden geavanceerde remediation acties die opnieuw instellen van wachtwoorden en meervoudige verificatie afdwingen bevatten.  

####<a name="explore-identity-protections-capabilities"></a>De mogelijkheden van de beveiliging van de identiteit verkennen 

**Opsporen risico's en risicogevoelig gevonden accounts:**  

- 6 risico gebeurtenistypen met machine learning en heuristische regels detecteren 

- Gebruiker risico niveaus berekenen

- Leveren van aangepaste aanbevelingen algehele beveiliging houding verbeteren door het markeren van problemen



**Wordt onderzocht risico gebeurtenissen:**

- Meldingen voor gebeurtenissen risico verzenden

- Wordt onderzocht risico's met behulp van relevante en contextuele informatie

- Eenvoudige werkstromen om bij te houden onderzoeken leveren

- Eenvoudige toegang tot remediation acties zoals wachtwoorden opnieuw instellen



**Risico's gebaseerde voorwaardelijke clienttoegangsbeleid:**

- Beleid risicogevoelig gevonden aanmeldingen beperken door aanmeldingen blokkeren of het vereisen van meervoudige verificatie uitdagingen.

- Beleid blokkeren of secure risicogevoelig gevonden gebruikersaccounts

- Beleid voor gebruikers om te registreren voor meervoudige verificatie vereisen


## <a name="detection-and-risk"></a>Detectie en risico 's

### <a name="risk-events"></a>Risico 's

Risico's zijn gebeurtenissen die zijn gemarkeerd als verdacht door identiteitsbeveiliging en aangeven dat een identiteit mogelijk is geknoeid. Zie voor een volledige lijst van risico's, [typen risico gebeurtenissen gedetecteerd door Azure Active Directory identiteitsbeveiliging](active-directory-identityprotection-risk-events-types.md). 


### <a name="risk-level"></a>Risiconiveau

Het niveau van de risico's voor een onvoorziene gebeurtenis is een vermelding van de ernst van de risicogebeurtenis (hoog, normaal of laag). Het niveau van de risico's helpt gebruikers van de identiteitsbeveiliging prioriteit voorzien, zodat de acties die ze nodig zijn om te verminderen van het risico aan hun organisatie. De ernst van de onvoorziene gebeurtenis vertegenwoordigt sterkte van het signaal voorspellen inbreuk op identiteit, gecombineerd met de hoeveelheid ruis die dit meestal wordt kort beschreven. 

- **Hoog**: hoge betrouwbaarheid en hoge prioriteit onvoorziene gebeurtenis. Deze gebeurtenissen zijn sterke indicatoren die de gebruikers id kent en gebruikersaccounts beïnvloed direct moeten worden verholpen.

- **Gemiddeld**: hoge prioriteit, maar lagere betrouwbaarheid onvoorziene gebeurtenis, of vice versa. Deze gebeurtenissen zijn mogelijk schadelijke en gebruikersaccounts beïnvloed moeten worden verholpen.

- **Laag**: lage betrouwbaarheid en lage prioriteit onvoorziene gebeurtenis. Deze gebeurtenis een onmiddellijke actie niet mogelijk, maar in combinatie met andere risico's, voorschrijven dat de identiteit is is gehackt groot. 


![Risiconiveau] (./media/active-directory-identityprotection/01.png "Risiconiveau")

 

Risico's al zijn aangewezen **realtime**, of in achteraf worden verwerkt nadat de onvoorziene gebeurtenis al is plaatsen (offline). Momenteel de meeste risico gebeurtenissen in identiteitsbeveiliging offline worden berekend en weergegeven in de identiteitsbeveiliging binnen 2-4 uur. Terwijl geëvalueerd in realtime, de gebeurtenissen realtime risico wordt weergegeven in de identiteit beveiliging Console binnen 5-10 minuten.

Verschillende oudere clients ondersteunen momenteel niet realtime risico gebeurtenis detecteren en tegenhouden. Hierdoor kunnen niet aanmeldingen van deze clients worden gedetecteerd of kon in realtime.


## <a name="investigation"></a>Onderzoek
Uw reis tot en met identiteitsbeveiliging is meestal wordt gestart met het dashboard identiteitsbeveiliging. 

![Remediation] (./media/active-directory-identityprotection/1000.png "Remediation")

Het dashboard hebt die u toegang tot:
 
- Rapporten zoals **gebruikers die zijn gemarkeerd voor risico**, **risico's** en **problemen**
- Instellingen zoals de configuratie van uw **Beveiligingsbeleid voor apparaten**, **meldingen** en **registratie voor meervoudige verificatie**
 

Dit is meestal uw uitgangspunt voor onderzoek, dat wil het proces zeggen voor het controleren van de activiteiten, logboeken en andere relevante informatie die betrekking hebben op een onvoorziene gebeurtenis te bepalen of remediation of risicobeperking stappen nodig zijn, en hoe de identiteit is gehackt is en hoe de beschadigde identiteit is gebruikt.

U kunt uw onderzoeksactiviteiten naar de die Azure Active Directory-beveiliging per e-mail verzendt [meldingen](active-directory-identityprotection-notifications.md) verbinden.

De volgende secties vindt u met meer informatie en de stappen die zijn gerelateerd aan een onderzoek.  



## <a name="what-is-a-user-risk-level"></a>Wat is een risico gebruikersniveau?

Een gebruiker risiconiveau wordt aangegeven (hoog, normaal of laag) van de kans dat de gebruikers id meer veilig is. Dit is berekend op basis van de gebruiker risico gebeurtenissen die zijn gekoppeld aan de gebruikers id. 

De status van een onvoorziene gebeurtenis is **actief** of **gesloten**. Alleen risico gebeurtenissen die **actief** zijn bijdragen aan de gebruiker risico berekening. 

Het niveau van de risico's van de gebruiker wordt berekend met de volgende invoer:

- Actieve risico gebeurtenissen die invloed hebben op de gebruiker
- Risiconiveau van deze gebeurtenissen 
- Opgeven of remediation bewerkingen is geopend 

![Gebruiker risico 's] (./media/active-directory-identityprotection/1001.png "Gebruiker risico 's")



U kunt de gebruiker risico niveaus gebruiken om te maken van voorwaardelijke clienttoegangsbeleid als u wilt blokkeren risicogevoelig gevonden gebruikers niet aanmelden of zorgen dat ze hun wachtwoord veilig te wijzigen. 


## <a name="closing-risk-events-manually"></a>Risico's handmatig sluiten

In de meeste gevallen gaat u remediation acties zoals een veilig wachtwoord opnieuw instellen om te risico gebeurtenissen automatisch te sluiten. Echter dit niet altijd mogelijk.  
Dit is, bijvoorbeeld de hoofdletters/kleine letters, wanneer:

- Een gebruiker met actieve risico's is verwijderd
- Een onderzoek blijkt dat een gerapporteerde onvoorziene gebeurtenis heeft zijn kunnen worden uitgevoerd door de gebruiker legitiem

Omdat het risico gebeurtenissen die **actief** zijn bijdragen aan de gebruiker risico berekening, moet u wellicht handmatig een risiconiveau verlagen door het risico's handmatig te sluiten.  
In de loop van onderzoek kunt u deze acties om de status van een onvoorziene gebeurtenis te wijzigen:

![Acties] (./media/active-directory-identityprotection/34.png "Acties")

- **Oplossen** - als na een onvoorziene gebeurtenis onderzoek, die u nog een gedaan juiste remediation buiten identiteitsbeveiliging, en u denkt dat de onvoorziene gebeurtenis moet worden beschouwd, gesloten, de gebeurtenis markeren als opgelost. Gebeurtenissen van de risicogebeurtenis-status wordt ingesteld dat gesloten en de onvoorziene gebeurtenis wordt niet langer bijdragen aan gebruiker risico opgelost.

- **Markeren als fout-positieve** - In sommige gevallen kan u een onvoorziene gebeurtenis onderzoeken en u ontdekt dat deze onjuist is gemarkeerd als een risicogevoelig gevonden. U kunt het aantal die gevallen verminderen door te markeren van de onvoorziene gebeurtenis als fout-positieve. Hierdoor wordt de machine learning-algoritmen ter verbetering van de indeling van soortgelijke gebeurtenissen in de toekomst. De status van de fout-positieve gebeurtenissen wordt **gesloten** en ze niet meer wordt bijdragen aan gebruiker risico.

- **Negeren** - als u geen actie remediation niet genomen, maar het risico-gebeurtenis wilt worden verwijderd uit de lijst met actieve, kunt u een onvoorziene gebeurtenis negeren markeren en de gebeurtenisstatus wordt gesloten. Genegeerde gebeurtenissen bijdragen niet tot gebruiker risico. Deze optie moet ongebruikelijke omstandigheden alleen worden gebruikt. 

- **Opnieuw activeren** - risico gebeurtenissen die handmatig gesloten zijn (door te kiezen **oplossen**, **fout-positief**of **negeren**) kan worden gereactiveerd, de gebeurtenisstatus terug naar de **actieve**is ingesteld. Opnieuw geactiveerde risico gebeurtenissen bijdragen aan de gebruiker risico niveau berekening. Risico's gesloten via remediation (zoals een beveiligde wachtwoordherstel) kunnen niet opnieuw worden geactiveerd. 




**Het configuratiedialoogvenster gerelateerde openen**:

1. Klik op het blad **Azure AD identiteit beveiliging** onder **onderzoeken**, op **risico's**.

    ![Handmatige wachtwoord opnieuw instellen] (./media/active-directory-identityprotection/1002.png "Handmatige wachtwoord opnieuw instellen")

2. Klik in de lijst **risico's** op een risico.

    ![Handmatige wachtwoord opnieuw instellen] (./media/active-directory-identityprotection/1003.png "Handmatige wachtwoord opnieuw instellen")

2. Klik op het blad risico, met de rechtermuisknop op een gebruiker.

    ![Handmatige wachtwoord opnieuw instellen] (./media/active-directory-identityprotection/1004.png "Handmatige wachtwoord opnieuw instellen")



### <a name="closing-all-risk-events-for-a-user-manually"></a>Alle risico-gebeurtenissen voor een gebruiker sluiten handmatig

In plaats van handmatig afzonderlijk risico's voor een gebruiker sluiten, biedt Azure Active Directory identiteitsbeveiliging u ook een manier om alle gebeurtenissen voor een gebruiker met één klik sluiten.


![Acties] (./media/active-directory-identityprotection/2222.png "Acties")

Wanneer u op **alle gebeurtenissen negeren**, alle gebeurtenissen zijn gesloten en de desbetreffende gebruiker is niet langer risico.



## <a name="remediating-user-risk-events"></a>Knop gebruiker risico gebeurtenissen

Een remediation is een actie voor het beveiligen van een identiteit of een apparaat die eerder is verdachte of bekende gevaar lopen. Een actie remediation herstelt u de identiteit of het apparaat naar een veilige status en lost u vorige risico gebeurtenissen die zijn gekoppeld aan de identiteit of het apparaat.

Als u wilt een verhelpen gebruiker risico's, kunt u het volgende doen:

- Een secure wachtwoord opnieuw instellen op een gebruiker risico's handmatig verhelpen uitvoeren 

- Een gebruiker risico beveiligingsbeleid als u wilt beperken of een gebruiker risico gebeurtenissen automatisch verhelpen configureren

- Afbeelding van de geïnfecteerd apparaat opnieuw  


### <a name="manual-secure-password-reset"></a>Handmatige secure wachtwoorden opnieuw instellen

Een beveiligde wachtwoorden opnieuw instellen is een effectieve doorvoeren voor veel risico gebeurtenissen en wanneer dat wordt uitgevoerd, automatisch wordt gesloten deze risico's en wordt herberekend niveau van de risico's. U kunt het dashboard identiteitsbeveiliging gebruiken om een wachtwoord voor een risicogevoelig gevonden gebruiker opnieuw instellen. 

Het gerelateerde dialoogvenster biedt twee verschillende methoden om een wachtwoord opnieuw instellen:

**Wachtwoord opnieuw** - Selecteer **gebruiker moet het wachtwoord opnieuw instellen** zodat de gebruiker zelf herstellen als de gebruiker heeft geregistreerd voor meervoudige verificatie. Tijdens een van de gebruiker volgende aanmelden, worden de gebruiker voor het oplossen van een uitdaging meervoudige verificatie is vereist en vervolgens moest het wachtwoord wijzigen. Deze optie is niet beschikbaar als het gebruikersaccount nog niet is geregistreerde meervoudige verificatie.

**Tijdelijke wachtwoord** - Select **genereren een tijdelijk wachtwoord** naar onmiddellijk ongeldig het bestaande wachtwoord, en maak een nieuw tijdelijke wachtwoord voor de gebruiker. De nieuwe, tijdelijke wachtwoord verzenden naar een alternatief e-mailadres voor de gebruiker of naar de manager van de gebruiker. Omdat het wachtwoord tijdelijke is, wordt de gebruiker wordt gevraagd het wachtwoord bij het aanmelden te wijzigen.


![Beleid] (./media/active-directory-identityprotection/1005.png "Beleid")


**Het configuratiedialoogvenster gerelateerde openen**:

1. Klik op het blad **Azure AD identiteit beveiliging** op **gebruikers die zijn gemarkeerd voor risico**.

    ![Handmatige wachtwoord opnieuw instellen] (./media/active-directory-identityprotection/1006.png "Handmatige wachtwoord opnieuw instellen")


2. In de lijst met gebruikers, selecteert u een gebruiker met ten minste één risico's.

    ![Handmatige wachtwoord opnieuw instellen] (./media/active-directory-identityprotection/1007.png "Handmatige wachtwoord opnieuw instellen")


2. Klik op het blad gebruiker op **wachtwoord opnieuw instellen**.

    ![Handmatige wachtwoord opnieuw instellen] (./media/active-directory-identityprotection/1008.png "Handmatige wachtwoord opnieuw instellen")





## <a name="user-risk-security-policy"></a>Beveiligingsbeleid voor gebruiker risico

Een gebruiker risico-beveiligingsbeleid is een voorwaardelijke-beleid dat evalueert het risiconiveau van de aan een specifieke gebruiker en is van toepassing remediation en risicobeperking acties op basis van vooraf gedefinieerde voorwaarden en regels.


![Gebruikersbeleid ridk] (./media/active-directory-identityprotection/1009.png "Gebruikersbeleid ridk")


Azure AD-identiteit beveiliging kunt u de risicobeperking en herstellen van gebruikers die zijn gemarkeerd voor risico doordat u kunt beheren:

- Stel de gebruikers en groepen die het beleid is van toepassing op: 

    ![Gebruikersbeleid ridk] (./media/active-directory-identityprotection/1010.png "Gebruikersbeleid ridk")


- De gebruiker risico drempel op (laag, normaal of hoog) die het beleid activeert instellen: 

    ![Gebruikersbeleid ridk] (./media/active-directory-identityprotection/1011.png "Gebruikersbeleid ridk")


- Laat de besturingselementen worden afgedwongen als het beleid wordt:

    ![Gebruikersbeleid ridk] (./media/active-directory-identityprotection/1012.png "Gebruikersbeleid ridk")


- De status van uw beleid schakelen:

    ![Gebruikersbeleid ridk] (./media/active-directory-identityprotection/403.png "MFA registratie")


- Bekijken en de invloed van een wijziging voordat u deze activeert beoordelen:

    ![Gebruikersbeleid ridk] (./media/active-directory-identityprotection/1013.png "Gebruikersbeleid ridk")


Hiermee reduceert u een **hoge** drempelwaarde kiezen hoe vaak een beleid wordt geactiveerd en de gevolgen voor gebruikers geminimaliseerd.
Echter worden **laag** en **Normaal** gebruikers die zijn gemarkeerd voor risico van het beleid, dat niet mogelijk secure identiteiten of apparaten die eerder zijn verdachte of bekend is gehackt worden uitgesloten.

Bij het instellen van het beleid,

- Gebruikers die zich waarschijnlijk het genereren van een groot aantal ONWAAR-positieve (ontwikkelaars, beveiliging analisten) uitsluiten

- Gebruikers in landen waar het inschakelen van het beleid niet praktische is uitsluiten (bijvoorbeeld geen toegang tot de helpdesk)

- Gebruik een **hoge** drempelwaarde tijdens de eerste beleid implementeren, of als u moet uitdagingen zichtbaar voor eindgebruikers minimaliseren.

- Gebruik een **laag** drempel als uw organisatie is vereist voor betere beveiliging. Een **laag** drempel selecteren maakt u kennis met extra gebruiker aanmeldingsproblemen uitdagingen, maar betere beveiliging.

De aanbevolen standaardinstelling voor de meeste organisaties is voor het configureren van een regel voor een drempelwaarde **Gemiddeld** te vinden van een balans tussen bruikbaarheid en beveiliging.

Voor een overzicht van de gerelateerde gebruikerservaring, raadpleegt u:

- [Compromised account herstel stroom](active-directory-identityprotection-flows.md#compromised-account-recovery).  

- [Compromised account geblokkeerd stroom](active-directory-identityprotection-flows.md#compromised-account-blocked).  


**Het configuratiedialoogvenster gerelateerde openen**:

1. Klik op het blad **Azure AD identiteit beveiliging** , klikt u in de sectie **configureren** op **risico gebruikersbeleid**.

    ![Gebruikersbeleid ridk] (./media/active-directory-identityprotection/1009.png "Gebruikersbeleid ridk")






## <a name="mitigating-user-risk-events"></a>Beperkende gebruiker risico gebeurtenissen
Beheerders kunnen de beveiligingsbeleid van een gebruiker risico voorkomen dat gebruikers bij het aanmelden afhankelijk van het risiconiveau instellen. 

Blokkeren van een aanmelden:
 
- Hiermee voorkomt u dat het genereren van de nieuwe gebruiker risico-gebeurtenissen voor de desbetreffende gebruiker

- Hiermee kunnen beheerders handmatig een verhelpen de risico's dat dit gevolgen heeft de gebruikers id en een beveiligde staat te herstellen



## <a name="what-is-a-sign-in-risk-level"></a>Wat is een risiconiveau aanmeldingsproblemen?

Een risiconiveau aanmeldingsproblemen is een vermelding van de kans dat voor een specifieke aanmeldingsproblemen, iemand anders probeert om te verifiëren met de gebruikers id (hoog, normaal of laag). Het niveau van de aanmeldingsproblemen risico's op het moment van een aanmeldingsproblemen wordt geëvalueerd en acht risico's en indicatoren gedetecteerd realtime voor die specifieke aanmeldingsproblemen. 

## <a name="mitigating-sign-in-risk-events"></a>Beperkende aanmeldingsproblemen risico gebeurtenissen 
Een risicobeperking is een actie aan de mogelijkheid voor onbevoegden misbruik van een beschadigde identiteit of het apparaat de identiteit of het apparaat hersteld in een veilige staat te beperken. Een risicobeperking vorige aanmeldingsproblemen risico gebeurtenissen die zijn gekoppeld aan de identiteit of het apparaat niet is opgelost.

U kunt voorwaardelijke toegang in Azure AD identiteit beveiliging automatisch aanmeldingsproblemen risico's te beperken. Met dit beleid, kunt u het risiconiveau van de gebruiker of u de aanmeldingsproblemen wilt blokkeren risicogevoelig gevonden aanmeldingen of dat de gebruiker om uit te voeren meervoudige verificatie. Deze acties kunnen verhinderen dat onbevoegden misbruik van een gestolen identiteit om te leiden tot beschadiging en mogelijk wacht u enige tijd om te beveiligen van de identiteit. 


## <a name="sign-in-risk-security-policy"></a>Aanmeldingsproblemen risico beveiligingsbeleid

Een beleid aanmeldingsproblemen risico is een voorwaardelijke-beleid dat het risico aan een specifieke aanmeldingsproblemen evalueert en is van toepassing op basis van vooraf gedefinieerde voorwaarden en regels beperkingen.

![Aanmeldingsproblemen risico beleid] (./media/active-directory-identityprotection/1014.png "Aanmeldingsproblemen risico beleid")


Azure AD-identiteit beveiliging kunt u het beperken van risicogevoelig gevonden aanmeldingen doordat u kunt beheren:

- Stel de gebruikers en groepen die het beleid is van toepassing op: 

    ![Aanmeldingsproblemen risico beleid] (./media/active-directory-identityprotection/1015.png "Aanmeldingsproblemen risico beleid")


- De aanmeldingsproblemen risico drempel op (laag, normaal of hoog) die het beleid activeert instellen: 

    ![Aanmeldingsproblemen risico beleid] (./media/active-directory-identityprotection/1016.png "Aanmeldingsproblemen risico beleid")


- Laat de besturingselementen worden afgedwongen als het beleid wordt:  

    ![Aanmeldingsproblemen risico beleid] (./media/active-directory-identityprotection/1017.png "Aanmeldingsproblemen risico beleid")
    

- De status van uw beleid schakelen:

    ![MFA registratie] (./media/active-directory-identityprotection/403.png "MFA registratie")

- Bekijken en de invloed van een wijziging voordat u deze activeert beoordelen: 

    ![Aanmeldingsproblemen risico beleid] (./media/active-directory-identityprotection/1018.png "Aanmeldingsproblemen risico beleid")


### <a name="what-you-need-to-know"></a>Wat u moet weten

U kunt een beveiligingsbeleid aanmeldingsproblemen risico meervoudige verificatie vereisen configureren:

![Aanmeldingsproblemen risico beleid] (./media/active-directory-identityprotection/1017.png "Aanmeldingsproblemen risico beleid")

Echter veiligheidsredenen worden werkt deze instelling alleen voor gebruikers die zich al zijn geregistreerd voor meervoudige verificatie. Als de voorwaarde meervoudige verificatie vereisen voor een gebruiker die nog niet is geregistreerd voor meervoudige verificatie is voldaan, wordt de gebruiker is geblokkeerd. 

Als een goede gewoonte, als u wilt meervoudige verificatie vereisen voor risicogevoelig gevonden aanmeldingen, moet u:

1. Het [beleid voor meervoudige verificatie registratie](#multi-factor-authentication-registration-policy) voor de betreffende gebruikers inschakelen.
2. De betreffende gebruikers aan aanmelding vereisen in een niet-risicogevoelig gevonden sessie voor het uitvoeren van een registratie MFA

U deze stappen uitvoert, zorgt ervoor dat meervoudige verificatie is vereist voor een risicogevoelig gevonden aanmeldingsproblemen. 


### <a name="best-practices"></a>Aanbevolen procedures

 
Hiermee reduceert u een **hoge** drempelwaarde kiezen hoe vaak een beleid wordt geactiveerd en de gevolgen voor gebruikers geminimaliseerd.  
 
Deze sluit echter **laag** en **Normaal** aanmeldingen gemarkeerd om risico's van het beleid, dat onbevoegden gebruikmaakt van een beschadigde identiteit kan niet worden geblokkeerd. 

Bij het instellen van het beleid, 

- Voorkomen dat gebruikers die geen / geen meervoudige verificatie

- Gebruikers in landen waar het inschakelen van het beleid niet praktische is uitsluiten (bijvoorbeeld geen toegang tot de helpdesk)

- Gebruikers die zich waarschijnlijk het genereren van een groot aantal ONWAAR-positieve (ontwikkelaars, beveiliging analisten) uitsluiten

- Gebruik een **hoge** drempelwaarde tijdens de eerste beleid implementeren, of als u moet uitdagingen zichtbaar voor eindgebruikers minimaliseren.

- Gebruik een drempel **laag** als uw organisatie is vereist voor betere beveiliging. Een **laag** drempel selecteren maakt u kennis met extra gebruiker aanmeldingsproblemen uitdagingen, maar betere beveiliging.

De aanbevolen standaardinstelling voor de meeste organisaties is voor het configureren van een regel voor een drempelwaarde **Gemiddeld** te vinden van een balans tussen bruikbaarheid en beveiliging.

 
Het beleid aanmeldingsproblemen risico luidt als volgt:

- Toegepast op alle browserverkeer en aanmeldingen met moderne verificatie.
- Niet toegepast op toepassingen oudere beveiligingsprotocollen gebruiken door het eindpunt WS-Trust bij de federatieve IDP, zoals ADFS uit te schakelen.

De pagina **Risico's** in de identiteitsbeveiliging-console bevat alle gebeurtenissen:

- Dit beleid is toegepast op
- U kunt de activiteit te bekijken en bepalen of de actie nodig, of mislukt is 

Voor een overzicht van de gerelateerde gebruikerservaring, raadpleegt u:

- [Risicogevoelig gevonden aanmeldingsproblemen herstel](active-directory-identityprotection-flows.md#risky-sign-in-recovery) 

- [Risicogevoelig gevonden aanmeldingsproblemen geblokkeerd](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  

- [Meervoudige verificatie registratie tijdens een risicogevoelig gevonden aanmelden](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in)  





**Het configuratiedialoogvenster gerelateerde openen**:

1. Klik op **aanmelden risico beleid**op het blad **Azure AD identiteit beveiliging** , klikt u in de sectie **configureren** .

    ![Gebruikersbeleid ridk] (./media/active-directory-identityprotection/1014.png "Gebruikersbeleid ridk")





## <a name="multi-factor-authentication-registration-policy"></a>Meervoudige verificatie registratiebeleid

Azure meervoudige verificatie is een methode om te controleren wie u bent die het gebruik van meer dan alleen een gebruikersnaam en wachtwoord vereist. Een tweede laag van beveiliging op gebruiker aanmeldingen en transacties krijgen.  
Het is raadzaam dat u Azure meervoudige verificatie voor aanmelding invoegtoepassingen voor gebruiker, vereist omdat deze:

- Sterke verificatie met een aantal eenvoudige controleopties biedt

- Speelt een belangrijke rol bij het voorbereiden van uw organisatie beveiligen en herstellen van account compromissen

![Gebruikersbeleid ridk] (./media/active-directory-identityprotection/1019.png "Gebruikersbeleid ridk")



Zie voor meer informatie, [Wat Azure meervoudige verificatie is?](../multi-factor-authentication/multi-factor-authentication.md)


Azure AD-identiteit beveiliging kunt u het album-out van meervoudige verificatie registratie beheren door een beleid waarmee u kunt te configureren: 



- Stel de gebruikers en groepen die het beleid is van toepassing op: 

    ![MFA beleid] (./media/active-directory-identityprotection/1020.png "MFA beleid")



- Laat de besturingselementen worden afgedwongen als het beleid wordt::  

    ![MFA beleid] (./media/active-directory-identityprotection/1021.png "MFA beleid")


- De status van uw beleid schakelen:

    ![MFA beleid] (./media/active-directory-identityprotection/403.png "MFA beleid")

- De huidige registratiestatus weergeven: 

    ![MFA beleid] (./media/active-directory-identityprotection/1022.png "MFA beleid")


Voor een overzicht van de gerelateerde gebruikerservaring, raadpleegt u:

- [Meervoudige verificatie registratie stroom](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).  

- [Meervoudige verificatie registratie tijdens een risicogevoelig gevonden aanmeldingsproblemen](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in).  





**Het configuratiedialoogvenster gerelateerde openen**:

1. Klik op het blad **Azure AD identiteit beveiliging** , klikt u in de sectie **configureren** op **meervoudige verificatie registratie**.

    ![MFA beleid] (./media/active-directory-identityprotection/1019.png "MFA beleid")





## <a name="next-steps"></a>Volgende stappen

 - [Kanaal 9: Azure AD en identiteit weergeven: identiteit beveiliging Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)
 - [Typen risico gebeurtenissen gedetecteerd door Azure Active Directory identiteitsbeveiliging](active-directory-identityprotection-risk-events-types.md)
 - [Problemen die worden gedetecteerd door Azure Active Directory identiteitsbeveiliging](active-directory-identityprotection-vulnerabilities.md)
 - [Azure Active Directory identiteitsbeveiliging meldingen](active-directory-identityprotection-notifications.md)
 - [Azure Active Directory identiteitsbeveiliging playbook](active-directory-identityprotection-playbook.md)
 - [Azure Active Directory identiteitsbeveiliging woordenlijst](active-directory-identityprotection-glossary.md)

 - [Aanmeldingsproblemen ervaringen met Azure AD identiteit-beveiliging](active-directory-identityprotection-flows.md)

 - [Beveiliging van Azure Active Directory-identiteit inschakelen](active-directory-identityprotection-enable.md)
 - [Azure Active Directory identiteitsbeveiliging - hoe u gebruikers de blokkering opheffen](active-directory-identityprotection-unblock-howto.md)

 - [Aan de slag met Azure Active Directory identiteitsbeveiliging en Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)


