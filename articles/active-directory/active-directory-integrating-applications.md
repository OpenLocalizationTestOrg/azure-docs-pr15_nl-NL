<properties
   pageTitle="Toepassingen integreren met Azure Active Directory | Microsoft Azure"
   description="Meer informatie over het toevoegen, bijwerken of verwijderen van een toepassing in Azure Active Directory (Azure AD)."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/29/2016"
   ms.author="mbaldwin;bryanla" />

# <a name="integrating-applications-with-azure-active-directory"></a>Toepassingen integreren met Azure Active Directory

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Softwareontwikkelaars en software-als-een-service (SaaS) providers kunnen ontwikkelen commerciële cloudservices of bedrijfstoepassingen die kan worden geïntegreerd met Azure Active Directory (Azure AD) beveiligd aanmelden en autorisatie voor hun services op te geven. Als u wilt een toepassing of service integreren met Azure AD, moet een ontwikkelaar eerst de details over het gebruik van de toepassing met Azure AD via de portal van Azure klassieke registreren.

In dit artikel leest u hoe u kunt toevoegen, bijwerken of verwijderen van een toepassing in Azure AD. U wordt meer informatie over de verschillende soorten toepassingen die kunnen worden geïntegreerd met Azure AD, het configureren van uw toepassingen voor toegang tot andere bronnen zoals web API's en meer.

Zie voor meer informatie over de twee Azure AD-objecten die een geregistreerde toepassing en de relatie ertussen vertegenwoordigt, [toepassingen en Service Principal objecten](active-directory-application-objects.md); Zie meer informatie over de bestaande huisstijl richtlijnen die u gebruiken moet bij het ontwikkelen van toepassingen met Azure Active Directory, [Huisstijl richtlijnen voor geïntegreerde Apps](active-directory-branding-guidelines.md).

## <a name="adding-an-application"></a>Een toepassing toe te voegen

Een toepassing die wil gebruiken van de mogelijkheden van Azure AD moet eerst worden geregistreerd in een Azure AD-tenant. Registratie hierbij met Azure AD gedetailleerde informatie over de toepassing, zoals de URL die waar is gevonden, de URL voor het verzenden van antwoorden nadat een gebruiker is geverifieerd, de URI die aangeeft van de app, enzovoort.

Als u een webtoepassing dat ondersteuning moet aanmelden voor gebruikers in Azure AD maakt, kunt u de onderstaande instructies te volgen. Als uw toepassing moet referenties of machtigingen voor toegang tot een web API of vereist zijn voor gebruikers van andere Azure AD-tenants toegang toe, [het bijwerken van een toepassing](#updating-an-application) Zie de sectie kunt doorgaan met het configureren van uw toepassing toestaan.

### <a name="to-register-a-new-application-in-the-azure-classic-portal"></a>Een nieuwe toepassing registreren in de klassieke Azure-portal

1. Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com).

1. Klik op het Active Directory-pictogram in het linkermenu en klik vervolgens op in de gewenste map.

1. Klik op het bovenste menu op **toepassingen**. Als u geen apps zijn toegevoegd aan uw adreslijst, wordt deze pagina alleen weergegeven de toevoegen de koppeling van een App. Klik op de koppeling, of u kunt ook klikken op op de knop **toevoegen** op de opdrachtbalk.

1. Klik op de wat wilt u pagina, klikt u op de koppeling naar de **toepassing het ontwikkelen van mijn organisatie toevoegen**.

1. Klik op de uitleg ons over naar de pagina toepassing en moet u een naam voor uw toepassing en het type toepassing die u met Azure AD registreert geven.  U kunt kiezen uit een [WebClient toepassing](active-directory-dev-glossary.md#client-application) / [web resource/API](active-directory-dev-glossary.md#resource-server) -toepassing (mogelijk ook fungeren als beide) of een [native client](active-directory-dev-glossary.md#native-client) -toepassing. Wanneer u klaar bent, klikt u op in de rechterbenedenhoek van de pagina op het pijlpictogram.

1. Op de pagina van de App-eigenschappen en geef de aanmelding URL en de App-ID-URI als u bent een webtoepassing of alleen de omleiden URI voor een systeemeigen clienttoepassing registreert en kies het selectievakje in de hoek van de hand van de rechterbenedenhoek van de pagina.

1. Uw toepassing is toegevoegd en u komt dan op de pagina snel aan de slag voor uw toepassing. Afhankelijk van of uw toepassing een web of de bijbehorende toepassing is, ziet u verschillende opties voor het toevoegen van extra mogelijkheden in uw toepassing. Wanneer u uw toepassing hebt toegevoegd, kunt u beginnen met het bijwerken van uw toepassing zodat gebruikers kunnen zich aanmeldt, access web API's in andere toepassingen, of meerdere tenant-toepassing (waarmee andere organisaties voor toegang tot uw toepassing) configureren.

>[AZURE.NOTE] Standaard is de registratie van de gemaakte toepassingen geconfigureerd zodat gebruikers uit uw adreslijst te melden bij uw toepassing.

## <a name="updating-an-application"></a>Een toepassing bijwerken

Nadat uw toepassing is geregistreerd met Azure AD, moet deze kan worden bijgewerkt naar toegang bieden tot web API's, beschikbaar gemaakt in andere organisaties en meer. Deze sectie worden verschillende manieren waarin u moet mogelijk uw toepassing verder te configureren. We wordt eerst een goed beeld van het kader toestemming, die is het belangrijk om te begrijpen als u samenstelt resource/API-toepassingen die wordt worden gebruikt door clienttoepassingen ingebouwd door ontwikkelaars in uw organisatie of een andere organisatie.

Zie voor meer informatie over de manier waarop verificatie in Azure AD werkt, [Verificatie-scenario's voor Azure AD](active-directory-authentication-scenarios.md).

### <a name="overview-of-the-consent-framework"></a>Overzicht van het kader toestemming

Azure AD toestemming framework kunt u gemakkelijk ontwikkelen van meerdere tenant web en native clienttoepassingen die moeten toegang hebben tot web API's die zijn beveiligd met een Azure AD-tenant, verschilt van de waar de clienttoepassing is geregistreerd. Deze web API's kunt u de grafiek-API, Office 365 en andere Microsoft-services, naast uw eigen web API's. Het kader is gebaseerd op een gebruiker of beheerder toestemming verlenen aan een toepassing waarin u wordt gevraagd in de adreslijst, waarbij toegang tot directorygegevens worden geregistreerd.

Bijvoorbeeld als een web-clienttoepassing bellen van een Office 365 web API/resource-toepassing moet te lezen van agenda-informatie over de gebruiker, die gebruiker moet toestemming met de clienttoepassing. Nadat u toestemming wordt uitgedrukt, kunnen de clienttoepassing bellen van de Office 365-web API namens de gebruiker en gebruik van de agendagegevens naar wens.

Het kader toestemming is gebaseerd op OAuth 2.0 en de verschillende stromen, zoals autorisatie code verlenen en client referenties verlenen, met openbare of vertrouwelijke klanten. Met behulp van OAuth 2.0 Azure AD mogelijk te groot aantal verschillende grafiektypen clienttoepassingen, zoals op een telefoon, tablet, server of een webtoepassing maken en toegang krijgen tot de vereiste bronnen.

Zie voor meer gedetailleerde informatie over het kader toestemming, [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx)-, [Verificatie-scenario's voor Azure AD](active-directory-authentication-scenarios.md)- en Office 365-onderwerp [lidmaatschap verificatie met Office 365-API's](https://msdn.microsoft.com/office/office365/howto/common-app-authentication-tasks).

#### <a name="example-of-the-consent-experience"></a>Voorbeeld van de toestemming-ervaring

De volgende stappen wordt uitgelegd u hoe de toestemming werkt voor zowel de ontwikkelaar van toepassingen en de gebruiker optreden.

1. Klik op de web-clienttoepassing configuratiepagina in de portal van Azure klassieke instellen welke machtigingen van die de toepassing nodig met behulp van de vervolgkeuzelijsten in de machtigingen aan een besturingselement voor andere toepassingen.

    ![Machtigingen voor andere toepassingen](./media/active-directory-integrating-applications/permissions.png)

1. Houd rekening met dat van uw toepassing machtigingen zijn bijgewerkt, de toepassing wordt uitgevoerd en een gebruiker wordt deze gebruiken voor de eerste keer. Als de toepassing niet al een access- of vernieuwen verkregen heeft toegangstoken, de toepassing moet gaat u naar het eindpunt van de Azure AD autorisatie voor een vergunning-code die kan worden gebruikt in het bezit van een nieuwe toegang en vernieuwen token.

1. Als de gebruiker niet al is geverifieerd, wordt ze te melden bij een Azure AD gevraagd.

    ![Gebruiker of beheerder aanmelden bij Azure AD](./media/active-directory-integrating-applications/useradminsignin.png)

1. Wanneer de gebruiker heeft aangemeld, bepaalt Azure AD als de gebruiker moet een pagina toestemming worden weergegeven. Deze bepaling is gebaseerd op het feit of de gebruiker (of beheerder van hun organisatie) al de toepassing toestemming heeft heeft. Als toestemming al niet is toegewezen, wordt Azure AD krijgt de gebruiker voor toestemming en worden de vereiste machtigingen die vereist is voor werking weergegeven. De set machtigingen die wordt weergegeven in het dialoogvenster toestemming zijn hetzelfde als wat is geselecteerd in de machtigingen aan een besturingselement voor andere toepassingen in de portal van Azure klassieke.

    ![Toestemming gebruikerservaring](./media/active-directory-integrating-applications/userconsent.png)

1. Nadat de gebruiker toestemming verleent, wordt een autorisatiecode wordt geretourneerd in uw toepassing, die kan worden ingewisseld bij het aanschaffen van een toegangstoken en token vernieuwen. Zie het gedeelte [web Application naar de webpagina-API sectie](active-directory-authentication-scenarios.md#web-application-to-web-api) in [Verificatie-scenario's voor Azure AD](active-directory-authentication-scenarios.md)voor meer informatie over deze stroom.

### <a name="configuring-a-client-application-to-access-web-apis"></a>Een clienttoepassing voor toegang tot web API's configureren

In de volgorde voor een web/vertrouwelijke clienttoepassing kunnen deelnemen aan een vergunning verlenen stroom waarvoor verificatie is vereist (en een toegangstoken verkrijgen), moet deze veilige referenties instellen. De standaard-verificatiemethode wordt ondersteund door de Azure-portal is cliënt-ID + symmetric toets. In deze sectie wordt uitgelegd hoe de volgende configuratiestappen uit vereiste referenties op te geven de geheime sleutel van uw klant.

Daarnaast kunnen voordat een client kan een web-API van access die worden aangeboden door een toepassing voor de resource (ie: Azure AD Graph API), het kader toestemming zorgt ervoor dat de client wordt opgehaald van de machtigingen verlenen vereist, op basis van de machtigingen aangevraagd. Standaard kunt alle toepassingen machtigingen van Azure Active Directory (Graph API) en Azure Service Management-API, Azure AD 'inschakelen aanmeldingsgegevens en lees gebruikersprofiel' gemachtigd is standaard geselecteerd. Als de clienttoepassing wordt geregistreerd in een Office 365 Azure AD-tenant, wordt web API's en machtigingen voor SharePoint en Exchange Online ook zijn beschikbaar voor selectie. U kunt kiezen uit [twee soorten machtigingen](active-directory-dev-glossary.md#permissions) in de vervolgkeuzelijsten naast de gewenste web API:

- Toepassingsmachtigingen: Vereisten voor de clienttoepassing voor toegang tot het web API rechtstreeks als zelf (geen gebruikerscontext). Dit type machtiging dat beheerder toestemming nodig en is ook niet beschikbaar voor systeemeigen clienttoepassingen.

- Gedelegeerd machtigingen: Vereisten voor de clienttoepassing voor toegang tot het web API als de gebruiker die zijn aangemeld, maar met door de geselecteerde machtiging beperkte toegang. In dit type machtiging dat kan worden verleend door een gebruiker, tenzij de machtiging is geconfigureerd als beheerder toestemming vereisen.

#### <a name="to-add-credentials-or-permissions-to-access-web-apis"></a>Referenties of machtigingen voor toegang tot web API's toevoegen

1. Meld u aan bij de [Azure klassieke portal.](https://manage.windowsazure.com)

1. Klik op het Active Directory-pictogram in het linkermenu en klik op op de gewenste map.

1. Klik in het bovenste menu **toepassingen**op en klik vervolgens op de toepassing die u wilt configureren. De pagina snel aan de slag wordt met eenmalige aanmelding en andere configuratiegegevens weergegeven. Klik op de koppeling **configureren** boven aan de pagina, die u naar de pagina configuratie van de toepassing gaat.

1. Als u wilt toevoegen van een geheime sleutel voor de referenties van uw webtoepassing, schuif omlaag naar de sectie "Toetsen".  

    - Eerste op de "Select duur" vervolgmenu en selecteer een 1 of 2 jaar duur. 
    - Een nieuwe rij, met de "Geldig van" en "Vervalt op" datums ingevuld wordt weergegeven.
    - De meest rechtse kolom bevat de waarde van de sleutel, nadat u wijzigingen in de configuratie (hieronder) hebt opgeslagen. Zorg ervoor dat u keert u terug naar deze sectie en deze kopiëren nadat u opslaan raakt, zodat u deze voor gebruik in de clienttoepassing tijdens de verificatie tijdens runtime.

2. Als u wilt toevoegen machtigingen voor toegang tot resource API's vanaf de klant, schuif omlaag naar de sectie 'De machtigingen voor andere toepassingen'. 
    - Klik eerst op de knop 'Toepassing toevoegen'.
    - Gebruik de vervolgkeuzelijst 'Weergeven' om te selecteren van het type van bronnen die u kiezen wilt uit.
    - De eerste kolom, kunt u in de beschikbare resource toepassingen in de map waarin een web API te selecteren. Klik op de resource die u geïnteresseerd bent en klik op het vinkje in de rechterbenedenhoek.
    - Nadat u hebt geselecteerd, ziet u de resource die is toegevoegd aan de lijst 'De machtigingen voor andere toepassingen'.
    - Selecteer de gewenste machtigingen voor de clienttoepassing in de vervolgkeuzelijsten 'Toepassingsmachtigingen' en 'Gedelegeerde machtigingen'.

1. Klik op de knop **Opslaan** op de balk met opdrachten wanneer u klaar bent. Als u een sleutel voor uw toepassing hebt opgegeven, wordt dit ook gegenereerd nadat u op Opslaan klikt.

>[AZURE.NOTE] De machtigingen voor uw toepassing te klikken op de knop Opslaan ook automatisch worden ingesteld in uw adreslijst op basis van de machtigingen voor andere toepassingen die u hebt geconfigureerd.  U kunt deze Toepassingsmachtigingen weergeven door te kijken op het tabblad van de eigenschappen van toepassing.

### <a name="configuring-a-resource-application-to-expose-web-apis"></a>Een resource-toepassingen om weer te geven van web API's configureren

U kunt een web API ontwikkelen en ter beschikking clienttoepassingen door access [bereiken](active-directory-dev-glossary.md#scopes) en [rollen](active-directory-dev-glossary.md#roles)weergeeft. Een correct geconfigureerde web API is beschikbaar net als de andere Microsoft-website API's, waaronder de API van de grafiek en de Office 365-API worden gemaakt. Access bereiken en rollen zijn toegankelijk via uw [van toepassing manifest](active-directory-dev-glossary.md#application-manifest), dat wil zeggen een JSON-bestand met de configuratie van de identiteit van de toepassing.  

De volgende sectie wordt uitgelegd hoe u om weer te geven van access bereiken te wijzigen van de resource-toepassing manifest.

#### <a name="adding-access-scopes-to-your-resource-application"></a>Access bereiken toevoegen aan uw toepassing resource

1. Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com).

1. Klik op het Active Directory-pictogram in het linkermenu en klik op op de gewenste map.

1. Klik in het bovenste menu **toepassingen**op en klik vervolgens op de resource-toepassing die u wilt configureren. De pagina snel aan de slag wordt met eenmalige aanmelding en andere configuratiegegevens weergegeven.

1. Klik op de knop **beheren bestandenlijst** in de opdrachtenbalk en selecteer **manifest te downloaden**.

1. Open het bestand voor een manifest van de JSON-toepassing en "oauth2Permissions" knooppunt vervangen door het volgende JSON-fragment. Het codefragment van dit is een voorbeeld van de manier waarop om weer te geven van een bereik bekend als 'gebruikersimitatie", waarmee de eigenaar van een resource aan een clienttoepassing een type gedelegeerd toegang geven aan een resource. Zorg ervoor dat u de tekst en waarden voor uw eigen toepassing wijzigen:

        "oauth2Permissions": [
        {
            "adminConsentDescription": "Allow the application full access to the Todo List service on behalf of the signed-in   user",
            "adminConsentDisplayName": "Have full access to the Todo List service",
            "id": "b69ee3c9-c40d-4f2a-ac80-961cd1534e40",
            "isEnabled": true,
            "type": "User",
            "userConsentDescription": "Allow the application full access to the todo service on your behalf",
            "userConsentDisplayName": "Have full access to the todo service",
            "value": "user_impersonation"
            }
        ],

    De id-waarde moet een nieuwe gegenereerde GUID die u maakt met behulp van een [GUID generatie hulpmiddel](https://msdn.microsoft.com/library/ms241442%28v=vs.80%29.aspx) of via een programma. Hiermee geeft u een unieke id voor de machtiging die wordt weergegeven door het web API. Zodra de klant correct is ingesteld voor toegang tot uw web API en oproepen de web API aanvragen, wordt deze een token OAuth 2.0 JWT met de claim bereik (scp) is ingesteld op de waarde bovenstaande, in dit geval user_impersonation is presenteren.

    >[AZURE.NOTE] U kunt aanvullende bereiken later zo nodig kunt laten zien. Houd rekening met dat uw web API meerdere bereiken die is gekoppeld aan een aantal verschillende functies mogelijk zichtbaar. U kunt nu toegang tot het web API bepalen met behulp van de claim bereik (scp) in het ontvangen OAuth 2.0 JWT token.

1. De bijgewerkte JSON-bestand opslaan en upload deze door te klikken op de knop **beheren bestandenlijst** in de opdrachtenbalk, **manifest uploaden**, bladeren naar uw bijgewerkte manifest-bestand te selecteren en vervolgens te selecteren. Nadat u hebt geüpload, wordt uw web-API nu geconfigureerd voor gebruik door andere toepassingen in uw adreslijst.

#### <a name="to-verify-the-web-api-is-exposed-to-other-applications-in-your-directory"></a>Om te controleren of het web wordt API blootgesteld aan andere toepassingen in uw adreslijst

1. Klik in het bovenste menu **toepassingen**op, selecteer de gewenste clienttoepassing die u wilt configureren voor toegang tot het web API en klik vervolgens op **configureren**.

1. Schuif omlaag naar de sectie toegang tot andere toepassingen. Klik op de vervolgkeuzelijst Select-toepassing en is mogelijk om te selecteren van het web API dat u alleen een machtiging voor weergegeven. Selecteer de nieuwe machtiging in de vervolgkeuzelijst gedelegeerde machtigingen.

![Takenlijst machtigingen worden weergegeven](./media/active-directory-integrating-applications/listpermissions.png)

#### <a name="more-on-the-application-manifest"></a>Meer informatie over manifest van de toepassing
Manifest van de toepassing wordt daadwerkelijk fungeert als om voor het bijwerken van de Toepassingsentiteit, waarin alle kenmerken van een Azure AD-toepassing identiteit configuratie, inclusief de API access bereiken besproken. Voor meer informatie over de Toepassingsentiteit, raadpleegt u de [toepassing van de API Graph entiteit documentatie](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity). In deze vindt u volledige naslaginformatie over de toepassing entiteitleden kon u de machtigingen voor uw API opgeven:  

- het lid appRoles, dat wil een verzameling [AppRole](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type) entiteiten die kunnen worden gebruikt zeggen om de **Machtigingen van een toepassing** voor een web API definiëren  
- het lid oauth2Permissions, dat wil een verzameling [OAuth2Permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type) entiteiten die kunnen worden gebruikt zeggen om de **Gedelegeerde machtigingen** voor een web API definiëren

Voor meer informatie over de toepassing Raadpleeg manifest concepten in het algemeen, [informatie over de manifest van de Azure Active Directory-toepassing](active-directory-application-manifest.md).

### <a name="accessing-the-azure-ad-graph-and-office-365-apis"></a>Toegang krijgen tot de Office 365-API's en Azure AD-grafiek

Zoals eerder is vermeld dat/hebt u toegang tot API's op uw eigen resource-toepassingen, kunt u ook de clienttoepassing voor toegang tot de API's die worden aangeboden door Microsoft resources bijwerken.  De API Azure AD grafiek, die is 'Azure Active Directory' genoemd in de lijst met machtigingen voor andere toepassingen, is standaard voor alle toepassingen die zijn geregistreerd met Azure AD beschikbaar. Als u de clienttoepassing in een Azure AD-tenant die door Office 365 is ingericht registreert, kunt u ook alle machtigingen die worden aangeboden door de API's om de verschillende Office 365-informatiebronnen openen.

Voor een volledige discussie op bereiken van access die worden aangeboden door:  

- Azure AD Graph API, raadpleegt u de bereiken [machtiging | Grafische API concepten](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes) artikel.
- Office 365-API's, Zie het artikel [verificatie en machtiging met algemene toestemming Framework](https://msdn.microsoft.com/office/office365/howto/application-manifest) . Zie [uw Office 365-ontwikkelomgeving instellen](https://msdn.microsoft.com/office/office365/HowTo/setup-development-environment) voor de grotere discussie op het maken van een client-app die werkt naadloos met Office 365-API's samen.

>[AZURE.NOTE] Vanwege een beperking, kunnen systeemeigen clienttoepassingen alleen inbellen bij de API Azure AD-grafiek als ze de machtiging "Adreslijst van uw organisatie toegang" gebruiken.  Deze beperking geldt niet voor webtoepassingen.

### <a name="configuring-multi-tenant-applications"></a>Meerdere tenant toepassingen configureren

Bij het toevoegen van een toepassing Azure AD, kunt u uw toepassing alleen worden geopend door gebruikers in uw organisatie. U kunt ook uw toepassing kan worden gebruikt door gebruikers in externe organisaties. Dit soort twee toepassing heten één tenant en meerdere tenant-toepassingen. U kunt de configuratie van een toepassing voor één tenant zodat u een toepassing van meerdere tenant, die in deze sectie wordt beschreven hoe onder wijzigen.

Het is belangrijk te weten de verschillen tussen een enkel tenant en meerdere tenant-toepassing:  

- Een toepassing voor één tenant is bedoeld voor gebruik in een organisatie. Ze zijn meestal een lijn-of-business (LoB)-toepassing geschreven door een ontwikkelaar enterprise. Een toepassing voor één tenant alleen moet worden gebruikt door gebruikers in een bepaalde map, en daardoor deze alleen moet worden deze is ingericht in één map.
- Een toepassing meerdere tenant is bedoeld voor gebruik in veel organisaties. Een webtoepassing van de software-als-een-service (SaaS) meestal geschreven door een onafhankelijke softwareleverancier (ISV) zijn. Meerdere tenant toepassingen moeten worden deze is ingericht in elke map waar ze worden gebruikt, waarin gebruiker of beheerder toestemming om u te registreren, ondersteund via het kader van Azure AD toestemming nodig. Houd er rekening mee dat alle systeemeigen clienttoepassingen multi tenant al dan niet standaard zijn, die worden geïnstalleerd op van de eigenaar van de resource-apparaat. Zie het overzicht van de sectie toestemming Framework hierboven voor meer informatie in het kader toestemming.

#### <a name="enabling-external-users-to-grant-your-application-access-to-their-resources"></a>Externe gebruikers de toepassing toegang verlenen tot hun resources inschakelen

Als u een toepassing die u beschikbaar wilt maken naar uw klanten of partners buiten uw organisatie schrijft, moet u de definitie van de toepassing in de portal van Azure klassieke bijwerken.

>[AZURE.NOTE] Als u meerdere tenant inschakelt, moet u ervoor zorgen dat u de App-ID-URI van uw toepassing eigenaar in een domein geverifieerd. Bovendien moet de retour-URL beginnen met https://. Zie de [toepassing en Service Principal objecten](active-directory-application-objects.md)voor meer informatie.

Toegang tot uw app voor externe gebruikers inschakelen: 

1. Meld u aan bij de [Azure klassieke portal.](https://manage.windowsazure.com)

1. Klik op het Active Directory-pictogram in het linkermenu en klik op op de gewenste map.

1. Klik in het bovenste menu **toepassingen**op en klik vervolgens op de toepassing die u wilt configureren. De pagina snel aan de slag wordt met configuratieopties weergegeven.

1. Vouw de sectie **Meerdere tenant-toepassing configureren** van de werkbalk Snel starten en klik op de koppeling **configureert u deze nu** in de sectie toegang inschakelen. De eigenschappenpagina van de toepassing wordt weergegeven.

1. Klik op de knop **Ja** naast de toepassing meerdere tenant is en klik vervolgens op de knop **Opslaan** op de opdrachtbalk.

Nadat u de bovenstaande wijziging hebt aangebracht, kunnen gebruikers en beheerders in andere organisaties uw toepassing toegang verlenen tot hun directory en andere gegevens.

#### <a name="triggering-the-azure-ad-consent-framework-at-runtime"></a>Het kader van de Azure AD toestemming gedurende runtime activeert

Als u wilt gebruiken in het kader toestemming, moeten meerdere tenant-clienttoepassingen verificatie met OAuth 2.0 aanvragen. [Voorbeelden van code](https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multi-tenant) zijn beschikbaar om aan te geven hoe een webtoepassing, de bijbehorende toepassing of de server/demontoepassing aanvraagt autorisatiecodes en access tokens web API's bellen.

Uw webtoepassing mogelijk ook van dienst aanmelding voor gebruikers. Als u een aanmelding dienst, wordt naar verwachting dat de gebruiker wordt Klik op een registreren knop dat wordt redirect de browser naar de Azure AD-OAuth2.0 Autoriseer eindpunt of een eindpunt userinfo OpenID verbinding maken. Deze eindpunten toestaan de toepassing voor informatie over de nieuwe gebruiker door te controleren van de id_token. Na de aanmelding fase de gebruiker krijgt met een prompt toestemming vergelijkbaar is met de bovenstaande in het overzicht van de sectie toestemming Framework.

U kunt ook uw webtoepassing mogelijk ook van dienst een waarmee beheerders kunnen "registreren in mijn bedrijf". Dit probleem zou de gebruiker ook omleiden naar de Azure AD-OAuth 2.0 Autoriseer eindpunt. In dit geval echter u gevraagd of doorgeven = admin_consent parameter naar het eindpunt autoriseren afdwingen van de beheerder toestemming ervaring, waarbij de beheerder toestemming namens hun organisatie wilt verlenen. Alleen een gebruiker die met een account die bij de globale beheerdersrol hoort verifieert kan bieden toestemming; anderen wordt een foutbericht. Op succesvolle toestemming, bevat het antwoord admin_consent = true. Als een toegangstoken wisselen, ontvangt u ook een id_token die u informatie over de organisatie vindt, en de beheerder die zich voor uw toepassing aanmeldde wordt.

### <a name="enabling-oauth-20-implicit-grant-for-single-page-applications"></a>Inschakelen van OAuth verlenen 2.0 impliciete voor één pagina-toepassingen

Eén pagina van de toepassing (kuuroorden) zijn meestal gestructureerd met een front-end van JavaScript-dik die wordt uitgevoerd in de browser, welke van de toepassing web API teruggebeld beëindigen als u wilt de bedrijfslogica uitvoeren. Voor kuuroorden ingesloten in een Azure AD, gebruikt u OAuth 2.0 impliciete verlenen voor de verificatie van de gebruiker met Azure AD en een token waarmee u beveiligde gesprekken uit van de toepassing JavaScript-client naar de back-end-web API kunt verkrijgen. Nadat de gebruiker toestemming gekregen heeft, kan dit dezelfde verificatieprotocol worden gebruikt voor tokens als u wilt beveiligen oproepen tussen de client en andere web API-resources die zijn geconfigureerd voor de toepassing. Zie [Wat is de stroom OAuth2 impliciete verlenen in Azure Active Directory ](active-directory-dev-understanding-oauth2-implicit-grant.md)meer informatie over de impliciete autorisatie verlenen en help bepaalt u of het juiste voor uw scenario toepassing is.

Impliciete verlenen OAuth 2.0 is standaard uitgeschakeld voor toepassingen. U kunt OAuth 2.0 impliciete verlenen voor uw toepassing inschakelen door in te stellen de `oauth2AllowImplicitFlow`"' waarde in de [manifest van de toepassing](active-directory-application-manifest.md), dat wil zeggen een JSON-bestand met de configuratie van de identiteit van de toepassing.

#### <a name="to-enable-oauth-20-implicit-grant"></a>Impliciete verlenen OAuth 2.0 inschakelen 

1. Meld u aan bij de [Azure klassieke portal.](https://manage.windowsazure.com)
1. Klik op het **Active Directory** -pictogram in het linkermenu en klik op op de gewenste map.
1. Klik in het bovenste menu **toepassingen**op en klik vervolgens op de toepassing die u wilt configureren. De pagina snel aan de slag wordt met eenmalige aanmelding en andere configuratiegegevens weergegeven.
1. Klik op de knop **beheren bestandenlijst** in de opdrachtenbalk en selecteer **manifest te downloaden**.
Open het bestand voor een manifest van de JSON-toepassing en stel de waarde "oauth2AllowImplicitFlow" op "true". Standaard is het "false".

    `"oauth2AllowImplicitFlow": true,`

1. De bijgewerkte JSON-bestand opslaan en upload deze door te klikken op de knop **beheren bestandenlijst** in de opdrachtenbalk, **manifest uploaden**, bladeren naar uw bijgewerkte manifest-bestand te selecteren en vervolgens te selecteren. Zodra u hebt geüpload, wordt uw web API nu geconfigureerd voor het OAuth 2.0 impliciete verlenen gebruiken om gebruikers te verifiëren.


### <a name="legacy-experiences-for-granting-access"></a>Oudere ervaringen voor het toekennen van access

Dit onderwerp vindt de oudere toestemming ervaring vóór 12 maart 2014. Dit probleem nog steeds wordt ondersteund, en wordt hieronder beschreven. Voordat u de nieuwe functionaliteit voor, kan u alleen de volgende machtigingen verlenen:

- Meld u aan op gebruikers in hun organisatie

- Meld u op gebruikers en lees de van hun organisatie directory-gegevens (als alleen de toepassing)

- Meld u aan op gebruikers en gegevens lezen en schrijven van hun organisatie directory (als alleen de toepassing)

U kunt de stappen in de [Tenant multi-webtoepassingen ontwikkelen met Azure AD](https://msdn.microsoft.com/library/azure/dn151789.aspx) toegang voor nieuwe geregistreerd in Azure AD-toepassingen. Het is belangrijk te weten dat het nieuwe toestemming kader krachtiger-toepassingen geeft en ook kan gebruikers toe te staan deze toepassingen in plaats van alleen beheerders.

#### <a name="building-the-link-that-grants-access-for-external-users-legacy"></a>Samenstellen van de koppeling die verleent toegang voor externe gebruikers (verouderd)

In de volgorde voor externe gebruikers te registreren voor uw app met hun organisatie-account, moet u uw app om weer te geven van een knop die verwijzen naar de pagina op Azure AD waarmee ze om toegang te verlenen bijwerken. Richtlijnen voor deze registreren knop huisstijl wordt besproken in het onderwerp [Huisstijl richtlijnen voor het geïntegreerde toepassingen](active-directory-branding-guidelines.md) . Nadat de gebruiker worden toegewezen of wordt de toegang geweigerd, wordt de pagina Azure AD verlenen toegang tot de browser omleiden naar uw app met een reactie. Zie voor meer informatie over de toepassingseigenschappen van de [objecten toepassing en Service beginselen](active-directory-application-objects.md).

De access-pagina verlenen Azure AD is gemaakt, en kunt u een koppeling naar deze pagina van de configuratie van uw app in de portal van Azure klassieke zoeken. Als u naar de pagina configuratie, klikt u op de koppeling **toepassingen** in het bovenste menu van uw Azure AD-tenant, klik op de app die u wilt configureren en klik vervolgens op **configureren** in het bovenste menu van de pagina snel starten.

De koppeling voor uw toepassing ziet er als volgt: `http://account.activedirectory.windowsazure.com/Consent.aspx?ClientID=058eb9b2-4f49-4850-9b78-469e3176e247&RequestedPermissions=DirectoryReaders&ConsentReturnURL=https%3A%2F%2Fadatum.com%2FExpenseReport.aspx%3FContextId%3D123456`. De volgende tabel beschrijft de onderdelen van de koppeling:

|Parameter|Beschrijving|
|---|---|
|ClientId|Vereist. De Client-ID die u als onderdeel van het toevoegen van uw app verkregen.|
|RequestedPermissions|Optioneel. Toegangsniveau die uw app aanvraagt, die wordt weergegeven aan de gebruiker toegang van uw app. Als u niet opgeeft, standaard het gewenste toegangsniveau eenmalige aanmelding alleen. De andere opties zijn DirectoryReaders en DirectoryWriters. Zie de toepassing toegangsniveaus voor meer informatie over deze toegangsniveaus.|
|ConsentReturnUrl|Optioneel. De URL die u wilt dat de verlenen access-antwoord aan. Deze waarde moet URL-codering en moet onder hetzelfde domein als de URL van het antwoord geconfigureerd in de definitie van uw app. Als u niet opgeeft, wordt het antwoord van de toegang verlenen omgeleid naar de URL van uw geconfigureerde beantwoorden.|

Een ConsentReturnUrl los van de URL antwoord geven, kunt uw app willen implementeren afzonderlijk logica die het antwoord op een andere URL uit de antwoord-URL (die normaal verwerkt SAML-tokens voor aanmelding op) kan worden verwerkt. U kunt ook opgeven aanvullende parameters in de ConsentReturnURL codering URL; Deze wordt doorgegeven als tekenreeks queryparameters terug naar uw app bij omleiding.  Deze methode kan worden gebruikt voor het behoud van extra info of van uw app-aanvraag voor een access-verlenen koppelen aan het antwoord van Azure AD.

#### <a name="grant-access-user-experience-and-response-legacy"></a>Verlenen van toegang gebruikerservaring en antwoorden (verouderd)

Wanneer een toepassing wordt omgeleid naar de koppeling van de toegang verlenen, wordt er in de volgende afbeeldingen laten zien wat de gebruiker ervaart.

Als de gebruiker is niet al aangemeld, wordt deze gevraagd dat te doen:

![Meld u aan bij AAD](./media/active-directory-integrating-applications/signintoaad.png)

Als de gebruiker is geverifieerd, worden Azure AD de gebruiker omgeleid naar de pagina van de toegang verlenen:

![Toegang verlenen](./media/active-directory-integrating-applications/grantaccess.png)

>[AZURE.NOTE] Alleen de bedrijfsbeheerder van de externe organisatie kunt toegang verlenen tot uw app. Als de gebruiker de bedrijfsbeheerder van een is, krijgt deze de optie om e-mail te verzenden naar hun bedrijfsbeheerder aanvragen dat deze app toegang is verleend.

Nadat de klant heeft toegang voor de app door te klikken op toegang verlenen of als u ze hebt toegang geweigerd door te klikken op Annuleren, Azure AD een antwoord naar de ConsentReturnUrl of de URL van uw geconfigureerde beantwoorden stuurt. In dit antwoord bevat de volgende parameters:

|Parameter|Beschrijving|
|---|---|
|TenantId|De unieke ID van de organisatie in Azure AD die toegang voor uw app heeft verleend. Deze parameter wordt alleen worden opgegeven als u de klant toegang.|
|Toestemming|Als de app is toegang, of als de aanvraag is geweigerd geweigerd, wordt de waarde naar verleend worden ingesteld.|

Aanvullende parameters worden geretourneerd naar de app als ze zijn opgegeven als onderdeel van de ConsentReturnUrl codering URL. Hieronder ziet u een voorbeeld antwoord wordt verzonden naar een toegangsaanvraag verlenen waarmee wordt aangegeven de toepassing gemachtigd zijn en bevat een context-id die is opgegeven in het verzoek van de toegang verlenen: `https://adatum.com/ExpenseReport.aspx?ContextID=123456&Consent=Granted&TenantId=f03dcba3-d693-47ad-9983-011650e64134`.

>[AZURE.NOTE] De toegang verlenen antwoord, hebben geen een beveiligingstoken voor de gebruiker. de app moet Meld u aan de gebruiker afzonderlijk.

Hieronder ziet u een voorbeeld reactie op een aanvraag voor het verlenen van toegang die is geweigerd.`https://adatum.com/ExpenseReport.aspx?ContextID=123456&Consent=Denied`

#### <a name="rolling-app-keys-for-uninterrupted-access-to-the-graph-api-legacy"></a>Schuivend app toetsen voor continu toegang hebben tot de API Graph (verouderd)

Tijdens de levensduur van uw app moet u mogelijk de toetsen die u gebruikt wanneer u in Azure AD belt waar u een toegangstoken om te bellen van de grafiek-API wijzigen.  In het algemeen wijzigen toetsen worden onderverdeeld in twee categorieën: noodgevallen album over waar uw sleutel meer veilig is of een album via wanneer uw huidige sleutel is bijna verlopen. De volgende procedure moet worden gevolgd om uw app ononderbroken toegang bieden terwijl u uw sleutels (hoofdzakelijk voor het tweede geval) vernieuwen.

1. In de klassieke Azure portal, klikt u op uw directory-tenant, **toepassingen** in het bovenste menu, klik op de app die u wilt configureren. De pagina snel aan de slag wordt met eenmalige aanmelding en andere configuratiegegevens weergegeven.

1. Klik op **configureren** in het bovenste menu voor een overzicht van eigenschappen van uw app, en u ziet een lijst van uw sleutels.

1. Onder sleutels, klik op de vervolgkeuzelijst **Selecteer duur** wordt gemeld dat 1 of 2 jaren kiezen en klik vervolgens op **Opslaan** in de opdrachtenbalk. Hierdoor wordt een nieuw wachtwoord-toets voor de app gegenereerd. Kopieer deze nieuwe wachtwoord-toets. Op dit moment kan zowel de bestaande als nieuwe sleutel worden gebruikt door uw app voor een toegangstoken van Azure AD.

1. Ga terug naar uw app en de configuratie om te beginnen met het nieuwe wachtwoordsleutel bijwerken. Zie [de grafiek-API om de Query Azure AD gebruiken](https://msdn.microsoft.com/library/azure/dn151791.aspx) voor een voorbeeld van het waar deze update moet gebeuren.

1. U moet nu deze wijziging implementeren van over uw productieomgeving – bevestigt deze eerst op één serviceknooppunt alvorens in de rest.

1. Als u de update hebt voltooid in uw productie-implementatie, bent u gratis keert u terug naar de portal van Azure klassieke en verwijder de oude sleutel.

#### <a name="changing-app-properties-after-enabling-access-legacy"></a>App-eigenschappen van een Groove na het inschakelen van access (verouderd)

Zodra u toegang voor externe gebruikers naar uw app inschakelt, moet u mogelijk doorgaan om de eigenschappen van uw app in de portal van Azure klassieke te wijzigen. Klanten die al toegang tot uw app hebt toegekend voordat u de app wijzigingen worden echter niet raadpleegt u deze wijzigingen doorgevoerd wanneer details over de app in de portal van Azure klassieke weergeven. Zodra de app beschikbaar voor klanten wordt gemaakt, moet u Wees voorzichtig wanneer u bepaalde wijzigingen aanbrengt. Als u de App-ID-URI bijwerkt, wordt klikt u vervolgens uw bestaande klanten van wie toegang voordat u deze wijziging bijvoorbeeld kan niet aanmelden bij uw app via hun bedrijf of school-accounts.

Als u een wijziging in de RequestedPermissions aanbrengt aanvragen van een groter toegangsniveau, bestaande klanten met app-functionaliteit die nu maakt gebruik van nieuwe API-aanroepen met deze verhoogd mogelijk toegangsniveau toegang geweigerd antwoord van de grafiek-API krijgen.  Uw app moet voor deze gevallen en vraag of de klant om toegang te verlenen van toegang tot uw app met het verzoek voor een betere toegangsniveau.

## <a name="removing-an-application"></a>Verwijderen van een toepassing

In deze sectie wordt beschreven hoe een toepassing verwijderen uit uw Azure AD-tenant.

### <a name="removing-an-application-authored-by-your-organization"></a>Verwijderen van een toepassing waaraan wordt gewerkt door uw organisatie
Hierna ziet u de toepassingen die voor uw Azure AD-tenant worden weergegeven onder het filter 'Toepassingen mijn bedrijf eigenaar is van' op de hoofdpagina van "Toepassingen". In technische termen zijn dit de toepassingen die u handmatig via de portal van Azure klassieke, of via een programma via PowerShell of de grafiek-API geregistreerd. Specifieke taken worden zij vertegenwoordigd door een toepassing en Service Principal object in de tenant. Zie de [toepassing en Service Principal objecten](active-directory-application-objects.md) voor meer informatie.

#### <a name="to-remove-a-single-tenant-application-from-your-directory"></a>Een toepassing één tenant verwijderen uit uw adreslijst

1. Meld u aan bij de [Azure klassieke portal.](https://manage.windowsazure.com)

1. Klik op het Active Directory-pictogram in het linkermenu en klik vervolgens op in de gewenste map.

1. Klik in het bovenste menu **toepassingen**op en klik vervolgens op de toepassing die u wilt configureren. De pagina snel aan de slag wordt met eenmalige aanmelding en andere configuratiegegevens weergegeven.

1. Klik op de knop **verwijderen** op de balk met opdrachten.

1. Klik op **Ja** in het bevestigingsvenster.

#### <a name="to-remove-a-multi-tenant-application-from-your-directory"></a>Een toepassing meerdere tenant verwijderen uit uw adreslijst

1. Meld u aan bij de [Azure klassieke portal.](https://manage.windowsazure.com)

1. Klik op het Active Directory-pictogram in het linkermenu en klik vervolgens op in de gewenste map.

1. Klik in het bovenste menu **toepassingen**op en klik vervolgens op de toepassing die u wilt configureren. De pagina snel aan de slag wordt met eenmalige aanmelding en andere configuratiegegevens weergegeven.

1. Over de toepassing is meerdere tenant sectie, klikt u op **Nee**. Hiermee converteert de toepassing kan één tenant, maar de toepassing blijven in een organisatie die al heeft ingestemd toe.

1. Klik op de knop **verwijderen** op de balk met opdrachten.

1. Klik op **Ja** in het bevestigingsvenster.

### <a name="removing-a-multi-tenant-application-authorized-by-another-organization"></a>Verwijderen van een toepassing voor meerdere tenant geautoriseerd door een andere organisatie
Hierna ziet u een subset van de toepassingen die worden weergegeven onder het filter "Toepassingen mijn bedrijf heeft" op de pagina belangrijkste "toepassingen' voor uw Azure AD-tenant, specifiek de bouwstenen die niet worden vermeld onder de lijst 'Toepassingen mijn bedrijf eigenaar is van'. In de technische termen zijn deze meerdere tenant-toepassingen die zijn geregistreerd tijdens het toestemming. Specifieke taken ze krijgen alleen een Service Principal object in de tenant. Zie de [toepassing en Service Principal objecten](active-directory-application-objects.md) voor meer informatie.

Om te verwijderen van een tenant multi-toepassing toegang tot het telefoonboek van uw (na het heeft verleend toestemming), moet de bedrijfsbeheerder van het een Azure-abonnement om toegang tot en met de portal van Azure klassieke te verwijderen. U gewoon Navigeer naar de pagina configuratie van de toepassing en klik op de knop "Toegang beheren" onderaan. De bedrijfsbeheerder van het kunt ook de [Azure AD PowerShell-Cmdlets](http://go.microsoft.com/fwlink/?LinkId=294151) gebruiken om toegang te verwijderen.

## <a name="next-steps"></a>Volgende stappen

- Zie de [Richtlijnen voor geïntegreerde Apps huisstijl](active-directory-branding-guidelines.md) voor tips visuele instructies voor de app op.

- Zie voor meer informatie over de relatie tussen de toepassing en Service Principal objecten van een toepassing, [toepassing en Service Principal objecten](active-directory-application-objects.md).

- Meer informatie over de rol van de app manifest wordt afgespeeld, Zie [Wat is de manifest van de Azure Active Directory-toepassing?](active-directory-application-manifest.md)

- Zie de [woordenlijst voor ontwikkelaars van Azure AD](active-directory-dev-glossary.md) voor definities van de belangrijkste Azure Active Directory (AD) ontwikkelaars begrippen.

- Ga naar de [handleiding voor ontwikkelaars van Active Directory](active-directory-developers-guide.md) voor een overzicht van alle ontwikkelaars verwante inhoud.
