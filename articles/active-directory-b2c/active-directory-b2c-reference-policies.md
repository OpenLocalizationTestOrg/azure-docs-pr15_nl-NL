<properties
    pageTitle="Azure Active Directory-B2C: Extensible beleid framework | Microsoft Azure"
    description="Een onderwerp van het beleidskader extensible van Azure Active Directory B2C en het maken van verschillende soorten van beleid"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-extensible-policy-framework"></a>Azure Active Directory-B2C: Extensible beleid framework

## <a name="the-basics"></a>De basisbeginselen

Het beleidskader extensible van Azure Active Directory (Azure AD) B2C is core sterkte van de service. Beleidsregels beschrijven volledig consumer identiteit ervaringen zoals aanmelding, aanmelden of profiel bewerken. Bijvoorbeeld kunt een aanmelding beleid u aangeven van gedrag door de volgende instellingen te configureren:

- Accounttypen (accounts van sociale zoals Facebook of lokale accounts zoals e-mailadres) die consumenten te registreren voor de toepassing kunnen gebruiken.
- Kenmerken (bijvoorbeeld voornaam, postcode en schoen grootte) te verzamelen van de consumer tijdens het aanmelden.
- Gebruik van meervoudige verificatie.
- De en uiterlijk van alle aanmeldingspagina's.
- (Die manifesten als claims in een token) dat de toepassing ontvangt wanneer het beleid is voltooid worden uitgevoerd.

U kunt meerdere beleidsregels van verschillende typen in de tenant maken en gebruiken in uw toepassingen naar wens. Beleidsregels voor de hele toepassingen kunnen worden gebruikt. Hiermee kan ontwikkelaars definiëren en consumenten identiteit ervaringen met minimale of geen wijzigingen in hun code wijzigen.

Beleidsregels zijn beschikbaar voor gebruik via een eenvoudige ontwikkelaars-interface. Uw toepassing een beleid met een standaard HTTP-verificatie-verzoek (een Beleidsparameter doorgeven in het verzoek) activeert en een aangepaste token als antwoord ontvangt. Bijvoorbeeld het enige verschil tussen aanvraagt aanroepen van een aanmelding beleid en die aanroepen van een beleid aanmelden is de naam van het beleid in de parameter "p" queryreeks gebruikt:

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

Zie voor meer informatie over het beleid framework dit [blogbericht](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).

## <a name="create-a-sign-up-policy"></a>Aanmelding-beleid maken

Als u wilt inschakelen aanmelding op uw toepassing, moet u een aanmelding beleid maken. Dit beleid worden de mogelijkheden die consumenten wordt lopen tijdens het aanmelden en de inhoud van tokens die de toepassing voor een succesvolle aanmelding-ups ontvangt.

1. [Als volgt te werk om te navigeren naar het blad B2C functies op de Azure-portal](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klik op **beleidsregels voor aanmelding**.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. De **naam** bepaalt de naam van het registreren beleid gebruikt door de toepassing. Voer bijvoorbeeld "SiUp".
5. Klik op **identiteitsprovider** en selecteer 'E-registratie'. U kunt desgewenst ook sociale id-providers, selecteren als al hebt geconfigureerd. Klik op **OK**.
6. Klik op **aanmelden bij kenmerken**. Hier kunt u kenmerken die u wilt verzamelen van de consumer tijdens het aanmelden kiezen. Selecteer bijvoorbeeld "Land/regio", "Weergavenaam" en "Postcode". Klik op **OK**.
7. Klik op **claims van toepassing**. Hier kunt u claims die u als resultaat in de tokens verzonden terug naar de toepassing na een geslaagde aanmelding ervaring wilt kiezen. Selecteer bijvoorbeeld "Weergavenaam", "Identiteitsprovider", "Postcode", "Gebruiker is nieuw" en "Gebruikerswachtwoord Object-ID".
8. Klik op **maken**. Opmerking dat het beleid net hebt gemaakt, wordt weergegeven als '**B2C_1_SiUp**' (de **B2C\_1\_ ** fragment automatisch toegevoegd) in het blad **beleidsregels voor aanmelding** .
9. Open het beleid door te klikken op '**B2C_1_SiUp**'.
10. Selecteer "Contoso B2C app' in de vervolgkeuzelijst van **toepassingen** en `https://localhost:44321/` in de **antwoord URL / omleiden URI** vervolgkeuzelijst.
11. Klik op **nu uitvoeren**. Een nieuw browsertabblad wordt geopend en u kunt uitvoeren via de consumenten-ervaring van zich registreert voor uw toepassing.

    > [AZURE.NOTE]
    Het duurt omhoog minuten voor het beleid maken en updates zijn doorgevoerd.

## <a name="create-a-sign-in-policy"></a>Aanmeldingsproblemen-beleid maken

Als u wilt inschakelen aanmelden bij uw toepassing, moet u een beleid aanmeldingsproblemen maken. Dit beleid worden de mogelijkheden die consumenten wordt op succesvolle aanmeldingen lopen tijdens het aanmelden en de inhoud van tokens die de toepassing ontvangt.

1. [Als volgt te werk om te navigeren naar het blad B2C functies op de Azure-portal](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klik op **aanmelden beleid**.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. De **naam** bepaalt de naam van het aanmeldingsproblemen beleid gebruikt door de toepassing. Voer bijvoorbeeld "SiIn".
5. Klik op **identiteitsprovider** en selecteer 'Aanmelding bij lokale Account'. U kunt desgewenst ook sociale identiteitsprovider, selecteren als al hebt geconfigureerd. Klik op **OK**.
6. Klik op **claims van toepassing**. Hier kunt u claims die u als resultaat in de tokens verzonden terug naar de toepassing na een geslaagde aanmeldervaring wilt kiezen. Selecteer bijvoorbeeld "Weergavenaam", "Identiteitsprovider", "Postcode" en "Gebruikerswachtwoord Object-ID". Klik op **OK**.
7. Klik op **maken**. Opmerking dat het beleid net hebt gemaakt, wordt weergegeven als '**B2C_1_SiIn**' (de **B2C\_1\_ ** fragment automatisch toegevoegd) in het blad **beleid voor het aanmelden** .
8. Open het beleid door te klikken op '**B2C_1_SiIn**'.
9. Selecteer "Contoso B2C app' in de vervolgkeuzelijst van **toepassingen** en `https://localhost:44321/` in de **antwoord URL / omleiden URI** vervolgkeuzelijst.
10. Klik op **nu uitvoeren**. Een nieuw browsertabblad wordt geopend en u kunt uitvoeren via de consumenten-ervaring aan te melden bij uw toepassing.

    > [AZURE.NOTE]
    Het duurt omhoog minuten voor het beleid maken en updates zijn doorgevoerd.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Inschrijfformulier of aanmeldingsproblemen-beleid maken

Dit beleid verwerkt beide ervaringen voor consumenten aanmelding & aanmelden met een enkele configuratie. Consumenten zijn onder leiding van een omlaag het juiste pad (aanmelding of aanmelden), afhankelijk van de context. Hierin worden ook de inhoud van tokens die de toepassing na het succesvolle Aanmeldingsadres-ups of aanmeldingen ontvangt.  Een codevoorbeeld voor het registreren of aanmeldingsproblemen beleid is [beschikbaar als volgt](active-directory-b2c-devquickstarts-web-dotnet-susi.md).

1. [Als volgt te werk om te navigeren naar het blad B2C functies op de Azure-portal](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klik op **aanmelden of aanmelden beleid**.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. De **naam** bepaalt de naam van het registreren beleid gebruikt door de toepassing. Voer bijvoorbeeld "SiUpIn".
5. Klik op **identiteitsprovider** en selecteer 'E-registratie'. U kunt desgewenst ook sociale identiteitsprovider, selecteren als al hebt geconfigureerd. Klik op **OK**.
6. Klik op **aanmelden bij kenmerken**. Hier kunt u kenmerken die u wilt verzamelen van de consumer tijdens het aanmelden kiezen. Selecteer bijvoorbeeld "Land/regio", "Weergavenaam" en "Postcode". Klik op **OK**.
7. Klik op **claims van toepassing**. Hier kunt u claims die u als resultaat in de tokens verzonden terug naar de toepassing na een geslaagde inschrijfformulier of aanmeldingsproblemen ervaring wilt kiezen. Selecteer bijvoorbeeld "Weergavenaam", "Identiteitsprovider", "Postcode", "Gebruiker is nieuw" en "Gebruikerswachtwoord Object-ID".
8. Klik op **maken**. Opmerking dat het beleid net hebt gemaakt, wordt weergegeven als '**B2C_1_SiUpIn**' (de **B2C\_1\_ ** fragment automatisch toegevoegd) in het blad **Inschrijfformulier of aanmeldingsproblemen beleid** .
9. Open het beleid door te klikken op '**B2C_1_SiUpIn**'.
10. Selecteer "Contoso B2C app' in de vervolgkeuzelijst van **toepassingen** en `https://localhost:44321/` in de **antwoord URL / omleiden URI** vervolgkeuzelijst.
11. Klik op **nu uitvoeren**. Een nieuw browsertabblad wordt geopend en u kunt uitvoeren via de inschrijfformulier of aanmeldingsproblemen consumenten-ervaring, zoals deze is geconfigureerd.

    > [AZURE.NOTE]
    Het duurt omhoog minuten voor het beleid maken en updates zijn doorgevoerd.

## <a name="create-a-profile-editing-policy"></a>Een profiel bewerken beleid maken

Als u wilt inschakelen profiel bewerken op uw toepassing, moet u een beleid bewerken profiel te maken. Dit beleid worden de mogelijkheden die consumenten wordt lopen tijdens het bewerken van het profiel en de inhoud van tokens de toepassing krijgt voor succesvolle afronding.

1. [Als volgt te werk om te navigeren naar het blad B2C functies op de Azure-portal](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klik op **profiel bewerken beleid**.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. De **naam van** bepaalt het profiel bewerken die wordt gebruikt door uw toepassing beleidsnaam. Voer bijvoorbeeld "SiPe".
5. Klik op **identiteitsprovider** en selecteer "E-mailadres". U kunt desgewenst ook sociale identiteitsprovider, selecteren als al hebt geconfigureerd. Klik op **OK**.
6. Klik op de **kenmerken van een profiel**. Hier kiest u de kenmerken die de consument kan bekijken en bewerken. Selecteer bijvoorbeeld "Land/regio", "Weergavenaam" en "Postcode". Klik op **OK**.
7. Klik op **claims van toepassing**. Hier kunt u claims die u als resultaat in de tokens verzonden terug naar de toepassing na een geslaagde profiel bewerken ervaring wilt kiezen. Selecteer bijvoorbeeld 'Weergavenaam' en 'Postcode'.
8. Klik op **maken**. Opmerking dat het beleid net hebt gemaakt, wordt weergegeven als '**B2C_1_SiPe**' (de **B2C\_1\_ ** fragment automatisch toegevoegd) in het blad **profiel bewerken beleid** .
9. Open het beleid door te klikken op '**B2C_1_SiPe**'.
10. Selecteer "Contoso B2C app' in de vervolgkeuzelijst van **toepassingen** en `https://localhost:44321/` in de **antwoord URL / omleiden URI** vervolgkeuzelijst.
11. Klik op **nu uitvoeren**. Een nieuw browsertabblad wordt geopend en u kunt uitvoeren via het profiel consumenten-ervaring in uw toepassing bewerken.

    > [AZURE.NOTE]
    Het duurt omhoog minuten voor het beleid maken en updates zijn doorgevoerd.
    
## <a name="create-a-password-reset-policy"></a>Een wachtwoord opnieuw instellen-beleid maken

Als u wilt fijnmazige wachtwoord opnieuw instellen op uw toepassing inschakelen, moet u een wachtwoord opnieuw instellen-beleid maken. Opmerking dat de hele tenant wachtwoordherstel optie opgegeven [hier](active-directory-b2c-reference-sspr.md) is nog steeds van toepassing op beleidsregels voor aanmelden. Dit beleid worden de ervaringen die de consumenten wordt lopen tijdens de wachtwoorden opnieuw instellen en de inhoud van tokens die de toepassing voor een succesvolle afronding ontvangt.

1. [Als volgt te werk om te navigeren naar het blad B2C functies op de Azure-portal](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klik op **wachtwoord opnieuw instellen van beleid**.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. De **naam** bepaalt dat het wachtwoord opnieuw instellen beleidsnaam gebruikt door de toepassing. Voer bijvoorbeeld "SSPR".
5. Klik op **identiteitsprovider** en selecteer 'Wachtwoord e-mailadres met'. Klik op **OK**.
6. Klik op **claims van toepassing**. Hier kunt u claims die u als resultaat in de tokens verzonden terug naar de toepassing nadat een succesvolle wachtwoordherstel ervaring wilt kiezen. Selecteer bijvoorbeeld "Gebruikerswachtwoord Object-ID".
7. Klik op **maken**. Opmerking dat het beleid net hebt gemaakt, wordt weergegeven als '**B2C_1_SSPR**' (de **B2C\_1\_ ** fragment automatisch toegevoegd) in het blad **wachtwoordherstel beleid** .
8. Open het beleid door te klikken op '**B2C_1_SSPR**'.
9. Selecteer "Contoso B2C app' in de vervolgkeuzelijst van **toepassingen** en `https://localhost:44321/` in de **antwoord URL / omleiden URI** vervolgkeuzelijst.
10. Klik op **nu uitvoeren**. Een nieuw browsertabblad wordt geopend en u kunt uitvoeren via het wachtwoord opnieuw instellen consumenten-ervaring in uw toepassing.

    > [AZURE.NOTE]
    Het duurt omhoog minuten voor het beleid maken en updates zijn doorgevoerd.

## <a name="additional-resources"></a>Aanvullende informatie

- [Token, sessie en configuratie voor eenmalige aanmelding](active-directory-b2c-token-session-sso.md).
