<properties
    pageTitle="Het inschakelen van de publicatie van native client-apps gebruiken met proxy-toepassingen | Microsoft Azure"
    description="Behandelt het inschakelen van native client-apps kunt u communiceren met Azure AD-toepassing Proxy verbindingslijn plaatsen, beveiligde externe toegang bieden tot uw on-premises implementatie-apps."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a>Het inschakelen van native client apps om te communiceren met proxy toepassingen

Azure Active Directory-toepassingsproxy wordt veel gebruikt browsertoepassingen zoals SharePoint, Outlook Web Access en aangepaste lijn bedrijfstoepassingen publiceren. Deze kan ook worden gebruikt voor het publiceren van native client-apps, waarin van de WebApps, verschillen omdat ze geïnstalleerd op een apparaat. Dit gebeurt door de ondersteuning van Azure AD uitgegeven tokens die worden verzonden in standard Autoriseer HTTP-headers.

![Relatie tussen eindgebruikers, Azure Active Directory en gepubliceerde toepassingen](./media/active-directory-application-proxy-native-client/richclientflow.png)

De aanbevolen methode voor het publiceren van dergelijke toepassingen is het gebruik van de Azure AD-verificatie is geopend, waarin zorgt voor alle de verificatie moeite en biedt ondersteuning voor veel verschillende client-omgevingen. Toepassingsproxy past in de [Bijbehorende toepassing Web API scenario](active-directory-authentication-scenarios.md#native-application-to-web-api). Het proces voor het uitvoeren van dit is als volgt:

## <a name="step-1-publish-your-application"></a>Stap 1: Uw toepassing publiceren

Publiceer uw proxy-toepassing zoals u zou doen met een andere toepassing, toewijzen aan gebruikers en ze bieden premium of eenvoudige licenties. Zie [publiceren toepassingen met toepassingsproxy](active-directory-application-proxy-publish.md)voor meer informatie.

## <a name="step-2-configure-your-application"></a>Stap 2: Configureer uw toepassing

Configureer uw oorspronkelijke toepassing als volgt:

1. Meld u aan bij de portal van Azure klassieke.
2. Selecteer het Active Directory-pictogram in het linkermenu en selecteer vervolgens de map.
3. Klik op het bovenste menu op **toepassingen**. Als u geen apps zijn toegevoegd aan uw adreslijst, worden deze pagina alleen de koppeling **een App toevoegen** weergegeven. Klik op de koppeling, of u kunt ook klikken op op de knop **toevoegen** op de opdrachtbalk.
4. Klik op de pagina **Wat wilt u doen** op de koppeling naar de **toepassing het ontwikkelen van mijn organisatie toevoegen**.
5. Klik op de pagina **Vertel ons over het gebruik van de toepassing** Geef een naam voor uw toepassing en kies **Native client-toepassing**. Klik op het pijlpictogram om door te gaan.
6. Op de informatiepagina van **toepassing** , de **URI omleiden** bieden voor de systeemeigen clienttoepassing en kies het vinkje om te voltooien.

Uw toepassing is toegevoegd en u komt dan op de pagina snel aan de slag voor uw toepassing.

## <a name="step-3-grant-access-to-other-applications"></a>Stap 3: Verlenen toegang tot andere toepassingen

De oorspronkelijke toepassing worden blootgesteld aan andere toepassingen in uw adreslijst inschakelen:

1. Klik in het bovenste menu **toepassingen**op, selecteert u de nieuwe systeemeigen toepassing en klik vervolgens op **configureren**.
2. Schuif omlaag naar de sectie **machtigingen voor andere toepassingen** . Klik op de knop **toevoegen-toepassing** en selecteer de proxy-toepassing die u wilt de bijbehorende toepassing toegang verlenen tot en klik op het vinkje in de rechterbenedenhoek. Selecteer de nieuwe machtiging in het vervolgkeuzemenu **Gedelegeerde machtigingen** .

![Machtigingen voor andere toepassingen schermafbeelding - toevoegen toepassing](./media/active-directory-application-proxy-native-client/delegate_native_app.png)

## <a name="step-4-edit-the-active-directory-authentication-library"></a>Stap 4: De Active Directory-verificatie-bibliotheek bewerken

De bijbehorende toepassingscode bewerken in de context van de verificatie van de Active Directory verificatie bibliotheek (ADAL) de volgende:

        // Acquire Access Token from AAD for Proxy Application
        AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<TenantId>");
        AuthenticationResult result = authContext.AcquireToken("< Frontend Url of Proxy App >",
                                                        "< Client Id of the Native app>",
                                                        new Uri("< Redirect Uri of the Native App>"),
                                                        PromptBehavior.Never);

        //Use the Access Token to access the Proxy Application
        HttpClient httpClient = new HttpClient();
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
        HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");

De variabelen wordt als volgt vervangen:

- **TenantId** vindt u in de GUID in de URL van de pagina van de **configuratie** van de toepassing, direct na '/ Directory /'.
- **Frontend URL** is de URL van de front-end van u zijn ingevoerd in de Proxy-toepassing en kan worden gevonden op de pagina **configuratie** van de proxy-app.
- **Cliënt-Id** van de systeemeigen app vindt u op de pagina **configureren** van de bijbehorende toepassing.
- **URI omleiden van de systeemeigen app** vindt u op de pagina **configureren** van de bijbehorende toepassing.

![Nieuwe systeemeigen toepassing pagina schermafbeelding configureren](./media/active-directory-application-proxy-native-client/new_native_app.png)

Zie [de toepassing waarmee web API](active-directory-authentication-scenarios.md#native-application-to-web-api)voor meer informatie over de stroom van de bijbehorende toepassing.


## <a name="see-also"></a>Zie ook

- [Toepassingen met uw eigen domeinnaam publiceren](active-directory-application-proxy-custom-domains.md)
- [Voorwaardelijke toegang inschakelen](active-directory-application-proxy-conditional-access.md)
- [Werken met op claims-toepassingen](active-directory-application-proxy-claims-aware-apps.md)
- [Inschakelen voor eenmalige aanmelding](active-directory-application-proxy-sso-using-kcd.md)

Voor het laatste nieuws en updates, raadpleegt u de [toepassingsproxy-blog](http://blogs.technet.com/b/applicationproxyblog/)
