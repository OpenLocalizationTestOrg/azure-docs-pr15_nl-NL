<properties
   pageTitle="Informatie over de Manifest van de Azure Active Directory-toepassing | Microsoft Azure"
   description="Gedetailleerde licenties voor het manifest Azure Active Directory-toepassing, van een toepassing identiteit configuratie in een Azure AD-tenant aangeeft, en wordt gebruikt om u te helpen gemakkelijker OAuth autorisatie, toestemming ervaring en meer."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="dkershaw;bryanla"/>

# <a name="understanding-the-azure-active-directory-application-manifest"></a>Informatie over de manifest van de Azure Active Directory-toepassing

Toepassingen die met Azure Active Directory (AD integreren) moeten zijn geregistreerd bij een Azure AD-tenant, een permanente identiteit configuratie voor de toepassing. Deze configuratie wordt geraadpleegd gedurende runtime, het inschakelen van scenario's die een toepassing laten uitbesteden en verificatie/machtiging via Azure AD broker. Zie voor meer informatie over het model Azure AD-toepassing, de [toevoegen, bijwerken, en verwijderen van een toepassing] [ ADD-UPD-RMV-APP] artikel.

## <a name="updating-an-applications-identity-configuration"></a>Configuratie van de identiteit van een toepassing bijwerken

Er zijn daadwerkelijk meerdere opties beschikbaar voor het bijwerken van de eigenschappen van een toepassing identiteit configuratie, die in mogelijkheden en graden moeilijkheidsgraad verschillen, waaronder de volgende:

- De ** [Azure klassieke portal] [ AZURE-CLASSIC-PORTAL] Web gebruikersinterface** kunt u de meest voorkomende eigenschappen van een toepassing bijwerken. Dit is de snelste en de minste fout vatbaar manier bijwerken van uw toepassing eigenschappen, maar kunt u volledige toegang tot alle eigenschappen, zoals de volgende twee methoden.
- Voor meer geavanceerde scenario's waarin u nodig hebt om eigenschappen die niet beschikbaar zijn in de portal van Azure klassieke te werken, kunt u de **manifest van de toepassing**. Dit is de nadruk in dit artikel en in detail starten in de volgende sectie is beschreven.
- Het is ook mogelijk om te **schrijven van een toepassing die gebruikmaakt van de [Grafiek API] [ GRAPH-API] ** uw toepassing, waarvoor u de meest hoeveelheid bijwerken. Dit kan een aantrekkelijke optie zijn hoewel, als u software voor projectmanagement schrijft of moet de eigenschappen van de servicetoepassing regelmatig geautomatiseerd voortgezet bijwerken.

## <a name="using-the-application-manifest-to-update-an-applications-identity-configuration"></a>Gebruik van manifest van de toepassing van een toepassing identiteit configuratie bij te werken
Via de [portal van Azure klassieke][AZURE-CLASSIC-PORTAL], kunt u de configuratie van de identiteit van de toepassing, beheren door te downloaden en uploaden van een JSON bestand weergave, die een manifest van de toepassing wordt genoemd. Geen werkelijke bestand is opgeslagen in de adreslijst. Manifest van de toepassing is slechts een HTTP GET-bewerking op de entiteit Azure AD Graph API-toepassing en de upload is een HTTP PATCH bewerking op de Toepassingsentiteit.

Hierdoor om te weten over de indeling en eigenschappen van manifest van de toepassing, moet u verwijzen naar de grafiek API- [Toepassingsentiteit] [ APPLICATION-ENTITY] documentatie. Voorbeelden van updates die kunnen worden uitgevoerd hoewel manifest uploaden van de toepassing zijn:

- **Declare machtiging bereiken (oauth2Permissions)** die worden aangeboden door uw web API. Zie het onderwerp 'Weergeeft API's naar andere webtoepassingen' in [Toepassingen integreren met Azure Active Directory] [ INTEGRATING-APPLICATIONS-AAD] voor informatie over het implementeren van gebruikersimitatie gebruik van de oauth2Permissions gedelegeerde machtiging bereik. Zoals eerder is vermeld, toepassing entiteitseigenschappen worden beschreven in de grafiek API [entiteit en complexType] [ APPLICATION-ENTITY] naslagartikel, met inbegrip van de eigenschap oauth2Permissions, dat wil een verzameling type [OAuth2Permission zeggen][APPLICATION-ENTITY-OAUTH2-PERMISSION].
- **Declare-toepassingsrollen (appRoles) die worden aangeboden door uw app**. Van de Toepassingsentiteit appRoles eigenschap is een verzameling type [AppRole][APPLICATION-ENTITY-APP-ROLE]. Zie de [Rolgebaseerd toegangsbeheer in de cloud-toepassingen met Azure AD] [ RBAC-CLOUD-APPS-AZUREAD] artikel voor een voorbeeld van de implementatie.
- **Declare bekende client toepassingen (knownClientApplications)**, waarmee u kunt de toestemming van de opgegeven client-toepassingen naar de resource/web API logisch te koppelen.
- **Azure AD naar actie-groep lidmaatschappen claimen aanvragen** voor de aangemeld gebruiker (groupMembershipClaims).  Dit kan ook worden geconfigureerd voor het verlenen van claims over van de gebruiker directory rollen lidmaatschappen. Zie de [autorisatie in Cloud-toepassingen gebruiken AD groepen] [ AAD-GROUPS-FOR-AUTHORIZATION] artikel voor een voorbeeld van de implementatie.
- **Toestaan dat uw toepassing voor de ondersteuning van OAuth 2.0 impliciete verlenen** loopt (oauth2AllowImplicitFlow). Dit soort verlenen stroom wordt gebruikt met ingesloten JavaScript webpagina's of één pagina toepassingen WACHTWOORDVERIFICATIE. Zie voor meer informatie over het verlenen impliciete autorisatie, [informatie over de impliciete OAuth2 verlenen stroom in Azure Active Directory][IMPLICIT-GRANT].
- **Gebruik van de inschakelen van X509 certificaten als de geheime sleutel** (keyCredentials). Zie het [bouwen van service en daemon apps in Office 365] [ O365-SERVICE-DAEMON-APPS] en [handleiding voor ontwikkelaars naar auth met Azure resourcemanager API] [ DEV-GUIDE-TO-AUTH-WITH-ARM] artikelen voor implementatie voorbeelden.
- **Een nieuwe App-ID-URI toevoegen** voor uw toepassing (identifierURIs[]). App-ID-URI's worden gebruikt als unieke aanduiding voor een toepassing binnen de Azure AD-tenant (of meerdere Azure AD-tenants, voor meerdere tenant scenario's wanneer gekwalificeerd via geverifieerd aangepaste domein). Deze worden gebruikt bij het aanvragen van machtigingen voor een toepassing voor de resource, of bij het verkrijgen van een toegangstoken voor een toepassing voor de resource. Wanneer u met dit element bijwerkt, worden dezelfde update wordt uitgevoerd naar de bijbehorende service hoofdsom van servicePrincipalNames [] siteverzameling, die zich in van de toepassing start tenant is ingeschakeld bevindt.

Manifest van de toepassing bevat ook een goede manier voor het bijhouden van de status van de registratie van uw toepassingen. Omdat het is beschikbaar in de indeling van JSON, kan de vorm van het bestand in het besturingselement voor gegevensbronnen, samen met de broncode van uw toepassing worden gecontroleerd.

## <a name="step-by-step-example"></a>Stap door stap voorbeeld
U kunt nu Doorloop de benodigde stappen voor het bijwerken van de configuratie van uw toepassing identiteit tot en met manifest van de toepassing. We worden gemarkeerd een van de voorgaande voorbeelden, laat zien hoe u een nieuwe machtiging scope op een toepassing voor de resource declareren:

1. Ga naar de [portal van Azure klassieke] [ AZURE-CLASSIC-PORTAL] en u aanmelden met een account met bevoegdheden voor service-beheerder of beheerder van collega.

2. Nadat u hebt geverifieerd, schuif omlaag en selecteert u de extensie Azure "Active Directory' in het linkernavigatievenster (1), klik vervolgens op registered de Azure AD-tenant waar uw toepassing is (2).

    ![Selecteer de Azure AD-tenant][SELECT-AZURE-AD-TENANT]

3. Wanneer de pagina directory verschijnt, klikt u op 'toepassingen"(1) boven aan de pagina om een lijst met toepassingen geregistreerd in de tenant weer te geven. Zoek de toepassing die u wilt bijwerken in de lijst en klik erop (2).

    ![Selecteer de Azure AD-tenant][SELECT-AZURE-AD-APP]

4. Nu dat u de hoofdpagina van de toepassing hebt geselecteerd, ziet u de functie "Beheren Manifest" vanaf de onderkant van de pagina (1). Als u op deze koppeling klikt, wordt u gevraagd om te downloaden of het manifest JSON-bestand uploaden. Klik op "Download Manifest" (2). Hiermee wordt onmiddellijk gevolgd met het downloaden bevestigingsvenster waarin u kunt bevestigen door te klikken op "Downloaden bestandenlijst" (3), en vervolgens op openen of opslaan lokaal het bestand (4).

    ![Het manifest beheren, optie downloaden][MANAGE-MANIFEST-DOWNLOAD]

    ![Het manifest downloaden][DOWNLOAD-MANIFEST]

5. In dit voorbeeld wordt het bestand hebt opgeslagen lokaal ons toestemming te openen in een editor, wijzigingen aanbrengen in de JSON en sla opnieuw. Hier ziet u hoe de JSON-structuur eruitziet in de Visual Studio JSON-editor. Opmerking dat het schema voor de [Toepassingsentiteit volgt] [ APPLICATION-ENTITY] zoals eerder vermeld:

    ![De bestandenlijst JSON bijwerken][UPDATE-MANIFEST]

    Bijvoorbeeld, ervan uitgaande dat we wilt implementeren/expose een nieuw machtigingsniveau "Employees.Read.All" met de naam op de resource-toepassing (API), u zou gewoon een nieuwe/tweede element toevoegen aan de collectie oauth2Permissions ie:

        "oauth2Permissions": [
        {
        "adminConsentDescription": "Allow the application to access MyWebApplication on behalf of the signed-in user.",
        "adminConsentDisplayName": "Access MyWebApplication",
        "id": "aade5b35-ea3e-481c-b38d-cba4c78682a0",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to access MyWebApplication on your behalf.",
        "userConsentDisplayName": "Access MyWebApplication",
        "value": "user_impersonation"
        },
        {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
        }
        ],

    De invoer moet uniek zijn en dus moet u een nieuwe globaal unieke ID (GUID) voor genereren de `"id"` eigenschap. In dit geval omdat we opgegeven `"type": "User"`, deze machtiging kan worden ingestemd met met elk account geverifieerd door de Azure AD-tenant in waarop de resource/API-toepassing is geregistreerd. Hiermee worden de client toegewezen machtigingen van de toepassing toegang toe van het account namens. De beschrijving en weergave naam tekenreeksen worden gebruikt tijdens toestemming en voor weergave in de portal van Azure klassieke.

6. Wanneer u klaar bent met het manifest bijwerken, terugkeren naar de pagina Azure AD-toepassing in de portal van Azure klassieke en klik op de "Beheren bestandenlijst" functie opnieuw (1). Maar ditmaal, selecteer de optie "Uploaden bestandenlijst" (2). Net zoals bij het downloaden, u wordt worden de volgende keer opnieuw met een tweede dialoogvenster waarin u wordt gevraagd naar de locatie van het JSON-bestand. Klik op "Zoeken naar een bestand..." (3), klikt u vervolgens het dialoogvenster "Kiest u bestand uploaden" om de JSON-bestand (4) te selecteren en druk op 'Openen'. Wanneer het dialoogvenster is verdwenen, selecteert u het selectievakje "OK" (5) en uw manifest worden geüpload.  

    ![Het manifest beheren, optie uploaden][MANAGE-MANIFEST-UPLOAD]

    ![De bestandenlijst JSON uploaden][UPLOAD-MANIFEST]

    ![De bestandenlijst JSON - bevestiging uploaden][UPLOAD-MANIFEST-CONFIRM]

Nu dat het manifest is opgeslagen, kunt u geven een geregistreerde client-toepassing toegang tot de nieuwe machtiging we hierboven hebt toegevoegd. Nu kunt u de Azure klassieke portal Web UI gebruiken in plaats van de clienttoepassing manifest bewerken:  

1. Eerst gaat u naar de pagina 'Configureren' van de clienttoepassing waarnaar u wilt toevoegen van toegang tot de nieuwe API en klik op de knop 'Toepassing toevoegen'.
2. Vervolgens wordt weergegeven met de lijst met geregistreerde resource-toepassingen (API's) in de tenant. Klik op het plusteken / + -teken naast de naam van de resource-toepassing om deze te selecteren.  
3. Klik vervolgens op het vinkje in de rechterbenedenhoek.
4. Wanneer u naar de sectie "Toepassing toevoegen" van de pagina configuratie van uw klant teruggaat, ziet u de nieuwe resource-toepassing in de lijst. Als u de muisaanwijzer op de sectie "Machtigingen gedelegeerde" aan de rechterkant van die rij, ziet u een vervolgkeuzelijst worden weergegeven. Klik op de lijst en selecteer vervolgens de nieuwe machtiging om te kunnen toevoegen aan de gevraagde clientlijst van machtigingen. Opmerking: deze nieuwe machtiging worden opgeslagen in de clienttoepassing identiteit configuratie wordt in de eigenschap van de siteverzameling "requiredResourceAccess".

![Machtigingen voor andere toepassingen][PERMS-TO-OTHER-APPS]

![Machtigingen voor andere toepassingen][PERMS-SELECT-APP]

![Machtigingen voor andere toepassingen][PERMS-SELECT-PERMS]

Dat is. Nu uw toepassingen uitgevoerd met de configuratie van hun nieuwe identiteit.

## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over de relatie tussen de toepassing en Service Principal objecten van een toepassing, [-toepassing en service principal objecten in Azure AD][AAD-APP-OBJECTS].
- Zie de [woordenlijst voor ontwikkelaars van Azure AD] [ AAD-DEVELOPER-GLOSSARY] voor definities van de belangrijkste Azure Active Directory (AD) ontwikkelaars begrippen.

Gebruik de onderstaande DISQUS opmerkingen sectie om feedback geven en help ons te verfijnen en de inhoud van vorm.

<!--Image references-->
[DOWNLOAD-MANIFEST]: ./media/active-directory-application-manifest/download-manifest.png
[MANAGE-MANIFEST-DOWNLOAD]: ./media/active-directory-application-manifest/manage-manifest-download.png
[MANAGE-MANIFEST-UPLOAD]: ./media/active-directory-application-manifest/manage-manifest-upload.png
[PERMS-SELECT-APP]: ./media/active-directory-application-manifest/portal-perms-select-app.png
[PERMS-SELECT-PERMS]: ./media/active-directory-application-manifest/portal-perms-select-perms.png
[PERMS-TO-OTHER-APPS]: ./media/active-directory-application-manifest/portal-perms-to-other-apps.png
[SELECT-AZURE-AD-APP]: ./media/active-directory-application-manifest/select-azure-ad-application.png
[SELECT-AZURE-AD-TENANT]: ./media/active-directory-application-manifest/select-azure-ad-tenant.png
[UPDATE-MANIFEST]: ./media/active-directory-application-manifest/update-manifest.png
[UPLOAD-MANIFEST]: ./media/active-directory-application-manifest/upload-manifest.png
[UPLOAD-MANIFEST-CONFIRM]: ./media/active-directory-application-manifest/upload-manifest-confirm.png

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-CLASSIC-PORTAL]: https://manage.windowsazure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

