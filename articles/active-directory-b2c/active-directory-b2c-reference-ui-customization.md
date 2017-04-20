<properties
    pageTitle="Azure Active Directory-B2C: Aanpassingen de gebruikersinterface (UI) | Microsoft Azure"
    description="Een onderwerp in de gebruiker gebruikersinterface (UI) aanpassingsfuncties in Azure Active Directory B2C"
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
    ms.date="07/25/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-customize-the-azure-ad-b2c-user-interface-ui"></a>Azure Active Directory-B2C: De Azure AD B2C-gebruikersinterface (UI) aanpassen

Gebruikerservaring is grootste in een toepassing voor consumenten-omlaag. Dit is het verschil tussen een toepassing voor goede en een fantastische en tussen alleen actieve consumenten en behoorlijk ingeschakeld. Azure Active Directory (Azure AD) B2C kunt u aanpassen op consumenten aanmelding, aanmelden (*Zie onderstaande notitie*), profiel bewerken, en wachtwoord opnieuw instellen voor pagina's met pixel-perfect besturingselement.

> [AZURE.NOTE]
Op dit moment lokale account aanmelden pagina's, het wachtwoord accompaning sitedefinitieversie van pagina's en verificatie-e-mailberichten kunnen worden aangepast dat alleen door met de [bestaande huisstijl functie bedrijf](../active-directory/active-directory-add-company-branding.md) en niet door de regelingen in dit artikel beschreven.

In dit artikel leest u over:

- De pagina gebruiker gebruikersinterface (UI) aanpassing functie.
- Een hulpmiddel waarmee u kunt de functie pagina UI aanpassen met de voorbeeldinhoud van onze testen.
- De core-gebruikersinterface-elementen in elk type pagina.
- Aanbevolen procedures bij deze functie.

## <a name="the-page-ui-customization-feature"></a>De pagina-functie voor het aanpassen van UI

U kunt het uiterlijk van consumenten aanmelding, aanmelden aanpassen met de functie pagina UI aanpassing, wachtwoorden opnieuw instellen en bewerken van profiel pagina's (door het [beleid](active-directory-b2c-reference-policies.md)configureren). Uw consumenten heeft naadloze oplossingen bij het navigeren tussen de toepassing en pagina's served door Azure AD B2C.

In tegenstelling tot andere services waarop UI-opties zijn beperkt of zijn alleen beschikbaar via de API's, gebruikt Azure AD B2C een modern (en eenvoudiger) benadering UI aanpassen van pagina's.

Hier ziet u hoe dat werkt: Azure AD B2C code in de browser van uw consument wordt uitgevoerd en gebruikmaakt van een modern aanpak genoemd [Cross-Origin Resource delen (CORS)](http://www.w3.org/TR/cors/) voor de inhoud van een URL die u in een beleid opgeeft laden. U kunt verschillende URL's voor verschillende pagina's opgeven. De code gebruikersinterface-elementen van Azure AD B2C samengevoegd met de inhoud van de URL geladen en wordt de pagina uw consumenten weergegeven. U hoeft te luidt als volgt:

1. Maak juist opgemaakte HTML5 inhoud met een `<div id="api"></div>` element (moet een leeg element) bevinden ergens in de `<body>`. In dit element markeringen waar de inhoud van de Azure AD B2C wordt ingevoegd.
2. Uw inhoud op een HTTPS-eindpunt hosten (met CORS toegestaan).
3. De gebruikersinterface-elementen die Azure AD B2C wordt ingevoegd in de stijl.

## <a name="test-out-the-ui-customization-feature"></a>Test u de functie voor het aanpassen van UI

Als u de functie voor het aanpassen van UI uitproberen wilt met behulp van onze steekproef HTML en CSS-inhoud, hebt we geleverd, kunt u [een eenvoudige helper hulpmiddel](active-directory-b2c-reference-ui-customization-helper-tool.md) dat uploads en Hiermee configureert u inhoud van de steekproef voor uw Azure-blobopslag.

> [AZURE.NOTE]
U kunt de inhoud van uw UI ergens hosten: op endwebservers, CDN's, AWS S3, bestand delen systemen, enzovoort. Zo lang maken als de inhoud wordt gehost op een openbaar beschikbaar HTTPS-eindpunt (met CORS toegestaan), bent u goed om te gaan. We gebruiken Azure-blobopslag voor alleen de illustratie.

### <a name="the-most-basic-html-content-for-testing"></a>De belangrijkste HTML-inhoud voor testdoeleinden

Hieronder wordt weergegeven, is de eenvoudigste HTML-code inhoud die u gebruiken kunt om te testen van deze mogelijkheid. Gebruik hetzelfde helper hulpmiddel eerder weergegeven om aan te uploaden en configureren van dit inhoudstype op uw Azure-blobopslag. U kunt vervolgens verifiëren dat de eenvoudige, niet-vorm van een gestileerde knoppen & formuliervelden die zijn op elke pagina weergegeven en functionele worden.

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>!Add your title here!</title>
    </head>
    <body>
        <div id="api"></div>    <!-- IMP: This element is intentionally empty; don't enter anything here -->
    </body>
</html>

```

## <a name="the-core-ui-elements-in-each-type-of-page"></a>De core gebruikersinterface-elementen in elk type pagina

In de volgende secties vindt u voorbeelden van HTML5 fragmenten die Azure AD B2C samengevoegd met de `<div id="api"></div>` element zich in uw inhoud. **Voeg deze fragmenten geen in de HTML 5-inhoud.** De service Azure AD B2C Hiermee voegt u deze in tijdens runtime. Gebruik deze voorbeelden voor het ontwerpen van uw eigen opmaakmodellen.

### <a name="azure-ad-b2c-content-inserted-into-the-identity-provider-selection-page"></a>Azure AD B2C inhoud op de "identiteit provider selectie pagina' ingevoegd

Deze pagina bevat een lijst met een identiteitsprovider die de gebruiker uit tijdens het aanmelden of aanmelden kiezen kunt. Hierna ziet u een sociale identiteitsprovider zoals Facebook en Google +, of een lokale accounts (gebaseerd op de naam van e-mailadres of de gebruiker).

```HTML

<div id="api" data-name="IdpSelections">
    <div class="intro">
         <p>Sign up</p>
    </div>

    <div>
        <ul>
            <li>
                <button class="accountButton" id="FacebookExchange">Facebook</button>
            </li>
            <li>
                <button class="accountButton" id="GoogleExchange">Google+</button>
            </li>
            <li>
                <button class="accountButton" id="SignUpWithLogonEmailExchange">Email</button>
            </li>
        </ul>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-local-account-sign-up-page"></a>Azure AD B2C inhoud ingevoegd in de "lokale account aanmeldpagina"

Deze pagina bevat een inschrijfformulier dat de gebruiker heeft in te vullen wanneer u zich registreert voor een lokale account die is gebaseerd op een e-mailadres of een gebruikersnaam in te voeren. Het formulier kan verschillende invoer besturingselementen zoals tekstinvoervak, wachtwoordinvoervak keuzerondje, één vervolgkeuzelijsten en meervoudige selectie selectievakjes bevatten.

```HTML

<div id="api" data-name="SelfAsserted">
    <div class="intro">
        <p>Create your account by providing the following details</p>
    </div>

    <div id="attributeVerification">
        <div class="errorText" id="passwordEntryMismatch" style="display: none;">The password entry fields do not match. Please enter the same password in both fields and try again.</div>
        <div class="errorText" id="requiredFieldMissing" style="display: none;">A required field is missing. Please fill out all required fields and try again.</div>
        <div class="errorText" id="fieldIncorrect" style="display: none;">One or more fields are filled out incorrectly. Please check your entries and try again.</div>
        <div class="errorText" id="claimVerificationServerError" style="display: none;"></div>
        <div class="attr" id="attributeList">
            <ul>
                <li>
                    <div class="attrEntry validate">
                        <div>
                            <div class="verificationInfoText" id="email_intro" style="display: inline;">Verification is necessary. Please click Send button.</div>
                            <div class="verificationInfoText" id="email_info" style="display:none">Verification code has been sent to your inbox. Please copy it to the input box below.</div>
                            <div class="verificationSuccessText" id="email_success" style="display:none">E-mail address verified. You can now continue.</div>
                            <div class="verificationErrorText" id="email_fail_retry" style="display:none">Incorrect code, try again.</div>
                            <div class="verificationErrorText" id="email_fail_no_retry" style="display:none">Exceeded number of retries you need to send new code.</div>
                            <div class="verificationErrorText" id="email_fail_server" style="display:none">Server error, please try again</div>
                            <div class="verificationErrorText" id="email_incorrect_format" style="display:none">Incorect format.</div>
                        </div>

                    <div class="helpText show">This information is required</div>
                        <label>Email</label>
                        <input id="email" class="textInput" type="text" placeholder="Email" required="" autofocus=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Email address that can be used to contact you.');" class="tiny">What is this?</a>

                    <div class="buttons verify" claim_id="email">
                        <div id="email_ver_wait" class="working" style="display: none;"></div>
                            <label id="email_ver_input_label" for="email_ver_input" style="display: none;">Verification code</label>
                            <input id="email_ver_input" type="text" placeholder="Verification code" style="display:none">
                            <button id="email_ver_but_send" class="sendButton" type="button" style="display: inline;">Send verification code</button>
                            <button id="email_ver_but_verify" class="verifyButton" type="button" style="display:none">Verify code</button>
                            <button id="email_ver_but_resend" class="sendButton" type="button" style="display:none">Send new code</button>
                            <button id="email_ver_but_edit" class="editButton" type="button" style="display:none">Change e-mail</button>
                            <button id="email_ver_but_default" class="defaultButton" type="button" style="display:none">Default</button>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ " ( ) ; .This information is required</div>
                        <label>Enter password</label>
                        <input id="password" class="textInput" type="password" placeholder="Enter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title="8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ &quot; ( ) ; ." required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Enter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"> This information is required</div>
                        <label>Reenter password</label>
                        <input id="reenterPassword" class="textInput" type="password" placeholder="Reenter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title=" " required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Reenter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Name</label>
                        <input id="displayName" class="textInput" type="text" placeholder="Name" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your display name.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Gender</label>
                        <input id="extension_Gender_F" name="extension_Gender" type="radio" value="F" autofocus="">
                        <label for="extension_Gender_F">Female</label>
                        <input id="extension_Gender_M" name="extension_Gender" type="radio" value="M">
                        <label for="extension_Gender_M">Male</label>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Loyalty number</label>
                        <input id="extension_MemNum" class="textInput" type="text" placeholder="Loyalty number"><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Membership number');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>State</label>
                        <select class="dropdown_single" id="state">
                            <option style="display:none" disabled="disabled" value="placeholder" selected="">State</option>
                            <option value="WA">Washington</option>
                            <option value="NY">New York</option>
                            <option value="CA">California</option>
                        </select>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your residential state or province.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Zip code</label>
                        <input id="postalCode" class="textInput" type="text" placeholder="Zip code" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('The postal code of your address.');" class="tiny">What is this?</a>
                    </div>
                </li>
            </ul>
        </div>
        <div class="buttons"> <button id="continue" disabled="">Create</button> <button id="cancel">Cancel</button></div>
    </div>
    <div class="verifying-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="verifying_blurb"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-social-account-sign-up-page"></a>Azure AD B2C inhoud ingevoegd in de "" sociale account aanmeldpagina"

Deze pagina bevat een inschrijfformulier waarop de consument in te vullen wanneer u zich aanmeldt met behulp van een bestaand account van een sociale identiteitsprovider zoals Facebook of Google +. Deze pagina lijkt op de lokale account aanmeldpagina (weergegeven in de vorige sectie) met uitzondering van de wachtwoordvelden.

### <a name="azure-ad-b2c-content-inserted-into-the-unified-sign-up-or-sign-in-page"></a>Azure AD B2C inhoud op de "geïntegreerde inschrijfformulier of aanmeldingsproblemen pagina ingevoegd"

Deze pagina omgaat met zowel aanmelding & aanmeldingsproblemen van consumenten, die de beschikking over sociale identiteitsprovider zoals Facebook of Google + of lokale accounts.

```HTML

<div id="api" data-name="Unified">
        <div class="social" role="form">
               <div class="intro">
                       <h2>Sign in with your social account</h2>
               </div>
               <div class="options">
                       <div><button class="accountButton firstButton" id="MicrosoftAccountExchange" tabindex="1">msa</button></div>
                       <div><button class="accountButton" id="FacebookExchange" tabindex="1">fb</button></div>
               </div>
        </div>
        <div class="divider">
               <h2>OR</h2>
        </div>
        <div class="localAccount" role="form">
               <div class="intro">
                       <h2>Sign in with your existing account</h2>
               </div>
               <div class="error pageLevel" aria-hidden="true" style="display: none;">
                       <p role="alert"></p>
               </div>
               <div class="entry">
                       <div class="entry-item">
                               <label for="logonIdentifier">Email Address</label> 
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="email" id="logonIdentifier" name="LogonIdentifier" pattern="^[a-zA-Z0-9.!#$%&amp;’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" placeholder="LogonIdentifier" value="" tabindex="1">
                       </div>
                       <div class="entry-item">
                               <div class="password-label"> <label for="password">Password</label><a id="forgotPassword" tabindex="2">Forgot your password?</a></div>
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="password" id="password" name="Password" placeholder="Password" tabindex="1">
                       </div>
                       <div class="working"></div>
                       <div class="buttons"> <button id="next" tabindex="1">Sign in</button> </div>
               </div>
               <div class="divider">
                       <h2>OR</h2>
               </div>
               <div class="create">
                       <p>Don't have an account?<a id="createAccount" tabindex="1">Sign up now</a> </p>
               </div>
        </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-multi-factor-authentication-page"></a>Azure AD B2C inhoud op de pagina' meervoudige verificatie' ingevoegd

Op deze pagina kunnen gebruikers hun telefoonnummers (met tekst of naar de voicemail) controleren tijdens het aanmelden of aanmelden.

```HTML

<div id="api" data-name="Phonefactor">
    <div id="phonefactor_initial">
        <div class="intro">
            <p>Enter a number below that we can send a code via SMS or phone to authenticate you.</p>
        </div>
        <div class="errorText" id="errorMessage" style="display:none"></div>
        <div class="phoneEntry" id="phoneEntry">
            <div class="phoneNumber">
                <select id="countryCode" style="display:inline-block">
                    <option value="+93">Afghanistan (+93)</option>
                    <!-- Not all country codes listed -->
                    <option value="+44">United Kingdom (+44)</option>
                    <option value="+1" selected="">United States (+1)</option>
                    <!-- Not all country codes listed -->
                </select>
            </div>
            <div class="phoneNumber">
                <input type="text" id="localNumber" style="display:inline-block" placeholder="Phone number">
            </div>
        </div>
        <div id="codeVerification" style="display:none">
            <div>
                <p>Enter your verification code below, or <button id="retryCode" class="textButton">send a new code</button></p>
                <input type="text" id="verificationCode" placeholder="Verification code">
            </div>
        </div>
        <div class="buttons">
            <button id="verifyCode" class="sendInitialCodeButton">Send Code</button>
            <button id="verifyPhone" style="display:inline-block">Call Me</button>
            <button id="cancel" style="display:inline-block">Cancel</button>
        </div>
    </div>
    <div class="dialing-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="dialing_blurb"></div><div id="dialing_number"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-error-page"></a>Azure AD B2C inhoud ingevoegd in de "" foutpagina"

```HTML

<div id="api" class="error-page-content" data-name="GlobalException">
    <h2>Sorry, but we're having trouble signing you in.</h2>
    <div class="error-page-help">We track these errors automatically, but if the problem persists feel free to contact us. In the meantime, please try again.</div>
    <div class="error-page-messagedetails">Your administrator hasn't provided any contact details.</div>
    <div class="error-page-messagedetails">
        <div class="error-page-correlationid">Correlation ID:1c4f0397-c6e4-4afe-bf74-42f488f2f15f</div>
        <div>Timestamp:2015-09-14 23:22:35Z</div>
        <div class="error-page-detail">AADB2C90065: A B2C client-side error 'Access is denied.' has occurred requesting the remote resource.</div>
    </div>
</div>

```

## <a name="things-to-remember-when-building-your-own-content"></a>Wat u moet onthouden wanneer u uw eigen inhoud maken

Als u van plan bent het gebruik van de pagina-functie voor het aanpassen van UI, raadpleegt u de volgende aanbevolen procedures:

- Niet kopiëren van de Azure AD-B2C standaardinhoud en probeert te wijzigen. Het is raadzaam voor het samenstellen van uw inhoud HTML5 helemaal en standaardinhoud gebruikt als referentie.
- Alle pagina's (behalve de fout pagina's) served door de aanmelden, aanmelden bij en het beleid profiel bewerken, opmaakmodellen die u opgeeft moet overschrijven de standaard-opmaakmodellen die we toevoegen aan deze pagina's in de <head> fragmenten. In alle pagina's served door de aanmelding of aanmelden en beleid van wachtwoorden opnieuw instellen en de fout pagina's op alle beleidsregels, hebt op te geven van alle de stijl zelf.
- Vanwege de beveiliging niet we u eventuele JavaScript opnemen in uw inhoud is toegestaan. Wat u zoekt de meeste zijn uit het vak beschikbaar. Als dat niet zo is, [Gebruiker Voice](http://feedback.azure.com/forums/169401-azure-active-directory) gebruikt voor het aanvragen van de nieuwe functionaliteit.
- Ondersteunde browserversies:
    - Internet Explorer 11
    - Internet Explorer 10
    - Internet Explorer 9 (beperkt)
    - Internet Explorer 8 (beperkt)
    - Google Chrome 43.0
    - Google Chrome 42.0
    - Mozilla Firefox 38.0
    - Mozilla Firefox 37.0
