<properties
    pageTitle="Hoe u een Web API-backend met Azure Active Directory en API Management beveiligen"
    description="Informatie over het beveiligen van een Web API-backend met Azure Active Directory en API-Management." 
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-protect-a-web-api-backend-with-azure-active-directory-and-api-management"></a>Hoe u een Web API-backend met Azure Active Directory en API Management beveiligen

De volgende video ziet hoe u een Web API-backend bouwen en beveiligen met OAuth 2.0-protocol met Azure Active Directory en API Management.  In dit artikel bevat een overzicht en aanvullende informatie voor de stappen in de video. Deze video 24 minuten ziet u hoe naar:

-   Een Web API-backend bouwen en beveiligt deze met AAD - beginnend bij 1:30
-   De API in beheer van de API - gerekend vanaf 7:10 importeren
-   De Developer portal om te bellen van de API - beginnend bij 9:09 configureren
-   Een bureaubladtoepassing de API - 18:08 vanaf aan te roepen configureren
-   Een beleid voor JWT validatie vooraf om aanvragen te verifiëren - 20:47 vanaf configureren

>[AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

## <a name="create-an-azure-ad-directory"></a>Een Azure AD-map maken

Beveiligen van uw Web-API back met Azure Active Directory die u eerst moet een een AAD-tenant. In deze video wordt een tenant die met de naam **APIMDemo** gebruikt. Als u wilt maken van een tenant AAD, aanmelden bij de [Klassieke Azure-Portal](https://manage.windowsazure.com) en klikt u op **Nieuw**->**App Services**->**Active Directory**->**Directory**->**Aangepaste maken**. 

![Azure Active Directory][api-management-create-aad-menu]

In dit voorbeeld wordt een map met de naam **APIMDemo** gemaakt met een standaarddomein met de naam **DemoAPIM.onmicrosoft.com**. Deze map wordt gebruikt in de video.

![Azure Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a>Een Web API-service beveiligd met Azure Active Directory maken

In deze stap is een Web API-backend gemaakt met Visual Studio-2013. In deze stap van de video begint bij 1:30. Maken van Web API backend-project in Visual Studio klikt u op **bestand**->**Nieuw**->**Project**en het selecteren van **ASP.NET-webtoepassing** uit de lijst van de **Web** -sjablonen. In deze video heet het project **APIMAADDemo**. Klik op **OK** om het project te maken. 

![Visual Studio][api-management-new-web-app]

Klik op **Web API** van het **selecteren van een lijst met sjablonen** een project Web API maken. U configureert op verificatie van Azure-Directory **Wijzigen verificatie**.

![Nieuw project][api-management-new-project]

Klik op **Organisatie-Accounts**en geef het **domein** van uw AAD-tenant. In dit voorbeeld is het domein **DemoAPIM.onmicrosoft.com**. Het domein van de map kan worden verkregen op het tabblad **domeinen** van uw adreslijst.

![Domeinen][api-management-aad-domains]

De gewenste instellingen configureren in het dialoogvenster **Verificatie wijzigen** en klik op **OK**.

![Verificatie wijzigen][api-management-change-authentication]

Wanneer u op **OK** Visual Studio probeert te registreren van uw toepassing met uw adreslijst Azure AD en wordt u mogelijk gevraagd aan te melden door Visual Studio. Meld u aan met een beheerdersaccount voor uw adreslijst.

![Meld u aan bij Visual Studio][api-management-sign-in-vidual-studio]

Dit project als een Azure Web API-Schakel het selectievakje configureren voor **Host in de cloud** en klik vervolgens op **OK**.

![Nieuw project][api-management-new-project-cloud]

Mogelijk wordt u gevraagd te melden bij een Azure en klikt u vervolgens kunt u de Web-App.

![Configureren][api-management-configure-web-app]

In dit voorbeeld wordt een nieuw **App-abonnement** met de naam **APIMAADDemo** opgegeven.

Klik op **OK** om de Web-App configureren en te maken van het project.

## <a name="add-the-code-to-the-web-api-project"></a>De code hebt toegevoegd aan het project Web API

De code wordt toegevoegd aan het project Web API van de volgende stap in de video. Deze stap 4:35 wordt gestart.

De Web-API in dit voorbeeld wordt een eenvoudige rekenmachine-service via een model en een controller geïmplementeerd. Als u wilt toevoegen in het model voor de service, met de rechtermuisknop op **modellen** in **Solution Explorer** en kies **toevoegen**, **Class**. Naam van de klas `CalcInput` en klik op **toevoegen**.

Voeg de volgende `using` instructie naar het begin van de `CalcInput.cs` bestand.

    using Newtonsoft.Json;

 Vervang de gegenereerde klasse door de volgende code.

    public class CalcInput
    {
        [JsonProperty(PropertyName = "a")]
        public int a;

        [JsonProperty(PropertyName = "b")]
        public int b;
    }

Met de rechtermuisknop op **domeincontrollers** in **Solution Explorer** en kies **toevoegen**->**Controller**. Kies **Web API 2 Controller - leegmaken** en klikt u op **toevoegen**. Typ **CalcController** voor de naam van de domeincontroller en klikt u op **toevoegen**.

![Controller toevoegen][api-management-add-controller]

Voeg de volgende `using` instructie naar het begin van de `CalcController.cs` bestand.

    using System.IO;
    using System.Web;
    using APIMAADDemo.Models;

De klas gegenereerde controller vervangen door de volgende code. Deze code wordt geïmplementeerd de `Add`, `Subtract`, `Multiply`, en `Divide` bewerkingen van de eenvoudige rekenmachine-API.

    [Authorize]
    public class CalcController : ApiController
    {
        [Route("api/add")]
        [HttpGet]
        public HttpResponseMessage GetSum([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a + b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/sub")]
        [HttpGet]
        public HttpResponseMessage GetDiff([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a - b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/mul")]
        [HttpGet]
        public HttpResponseMessage GetProduct([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a * b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/div")]
        [HttpGet]
        public HttpResponseMessage GetDiv([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a / b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }
    }

Druk op **F6** om te bouwen en controleer of de oplossing.

## <a name="publish-the-project-to-azure"></a>Het project publiceren naar Azure

In deze stap de Visual Studio wordt project gepubliceerd naar Azure. In deze stap van de video begint bij 5:45.

Als u het project wilt Azure publiceren, met de rechtermuisknop op het project **APIMAADDemo** in Visual Studio en kies **publiceren**. Behoud de standaardinstellingen in het dialoogvenster **Web publiceren** en klik op **publiceren**.

![Web publiceren][api-management-web-publish]

## <a name="grant-permissions-to-the-azure-ad-backend-service-application"></a>Machtigingen verlenen voor de Azure AD backend-servicetoepassing

Een nieuwe toepassing voor de backend-service is gemaakt in uw adreslijst Azure AD als onderdeel van het proces configureren en publiceren van uw project Web API. In deze stap van de video, beginnend bij 6:13, machtigingen op de backend Web API.

![Toepassing][api-management-aad-backend-app]

Klik op de naam van de toepassing voor het configureren van de vereiste machtigingen. Ga naar het tabblad **configureren** en schuif omlaag naar de sectie **machtigingen voor andere toepassingen** . Klik op de **Machtigingen van toepassing** -omlaag naast **Windows** **Azure Active Directory**, schakel het selectievakje voor **directory-gegevens lezen**en klik op **Opslaan**.

![Machtigingen toevoegen][api-management-aad-add-permissions]

>[AZURE.NOTE] Als **Windows** **Azure Active Directory** niet wordt vermeld onder machtigingen voor andere toepassingen, klik op **de toepassing toevoegen** en deze toevoegen in de lijst.

Maak een notitie van de **App-Id-URI** voor gebruik in een latere stap wanneer een Azure AD-toepassing is geconfigureerd voor de API Management developer portal.

![App-Id URI][api-management-aad-sso-uri]

## <a name="import-the-web-api-into-api-management"></a>Het importeren van het Web API in API Management

API's zijn geconfigureerd met de API publisher-portal, die wordt geopend met de klassieke Azure-Portal. Klik op **beheren** in de klassieke Azure-Portal voor uw API Management-service te bereiken de publisher-portal. Als u een exemplaar van de service API Management nog niet hebt gemaakt, raadpleegt u [een exemplaar van de service API Management maken][] in deze zelfstudie [beheren uw eerste API][] .

![Publisher-portal][api-management-management-console]

Bewerkingen kunnen worden [toegevoegd aan de API's handmatig](api-management-howto-add-operations.md)of ze kunnen worden geïmporteerd. In deze video worden bewerkingen geïmporteerd in Swagger opmaak, beginnend bij 6:40.

Maken van een bestand met de naam `calcapi.json` met de volgende onderwerpen en opslaan op uw computer. Zorg ervoor dat de `host` verwijzen naar uw Web API-backend kenmerk. In dit voorbeeld `"host": "apimaaddemo.azurewebsites.net"` wordt gebruikt.

{"swagger": '2.0', 'info': {"titel": "Rekenmachine", "Beschrijving": "Arithmetics via HTTP!", 'versie': "1.0"}, "host": "apimaaddemo.azurewebsites.net", "basePath": "/ api", "schema": ["http"], "paden": {"/ toevoegen? een = {a} & b = {b}": {"Haal": {"Beschrijving": "Reageert met een som van twee getallen.", "bewerkingsnummer": 'Toevoegen twee gehele getallen', 'parameters': [{"naam": 'a', 'in': "query", "Beschrijving": "eerste operand. Standaardwaarde is <code>51</code>. ","vereist": waar,"standaard":"51","opsommen": ["51"]}, {"naam":"b", 'in':"query","Beschrijving":"tweede operand. Standaardwaarde is <code>49</code>. ","vereist": waar,"standaard":"49","opsommen": ["49"]}],"antwoorden": {}}}," / sub?a = {a} & b = {b} ": {"Haal": {"Beschrijving":"Reageert met een verschil tussen twee getallen.","bewerkingsnummer": 'Aftrekken twee gehele getallen', 'parameters': [{"naam": 'a', 'in':"query","Beschrijving":"eerste operand. Standaardwaarde is <code>100</code>. ","vereist": waar,"standaard": '100', 'vaste': ['100']}, {"naam":"b", 'in':"query","Beschrijving":"tweede operand. Standaardwaarde is <code>50</code>. ","vereist": waar,"standaard":"50","opsommen": ["50"]}],"antwoorden": {}}}," / div?a = {a} & b = {b} ": {"Haal": {"Beschrijving":"Reageert met een quotiënt van twee getallen.","bewerkingsnummer": 'Twee gehele getallen delen', 'parameters': [{"naam": 'a', 'in':"query","Beschrijving":"eerste operand. Standaardwaarde is <code>100</code>. ","vereist": waar,"standaard": '100', 'vaste': ['100']}, {"naam":"b", 'in':"query","Beschrijving":"tweede operand. Standaardwaarde is <code>20</code>. ","vereist": waar,"standaard":"20,""opsommen": ["20"]}],"antwoorden": {}}}," / mul?a = {a} & b = {b} ": {"Haal": {"Beschrijving":"Reageert met een product van twee getallen.","bewerkingsnummer": 'Vermenigvuldigen twee gehele getallen', 'parameters': [{"naam": 'a', 'in':"query","Beschrijving":"eerste operand. Standaardwaarde is <code>20</code>. ","vereist": waar,"standaard":"20,""opsommen": ["20"]}, {"naam":"b", 'in':"query","Beschrijving":"tweede operand. Standaardwaarde is <code>5</code>. ","vereist": waar,"standaard": '5', 'vaste': ["5"]}],"antwoorden": {}}}}}

Als u wilt importeren de Rekenmachine API, **API's** in het menu **API beheer** aan de linkerkant op en klik vervolgens op **API importeren**.

![Knop importeren API][api-management-import-api]

De volgende stappen om te configureren de Rekenmachine API uitvoeren.

1. Klik op **uit bestand**, blader naar de `calculator.json` u opgeslagen bestand en klik op het keuzerondje **Swagger** .
2. Typ **berekenen** in het tekstvak **achtervoegsel Web API-URL** .
3. Klik in het vak **producten (optioneel)** en kies **Starter**.
4. Klik op **Opslaan** als u wilt importeren de API.

![Nieuwe API toevoegen][api-management-import-new-api]

Nadat de API zijn geïmporteerd, wordt de overzichtspagina voor de API in de publisher-portal weergegeven.

## <a name="call-the-api-unsuccessfully-from-the-developer-portal"></a>De API zich niet bellen vanuit de developer portal

Nu de API in API Management is ingevoerd, maar kan niet nog worden aangeroepen succes van de developer portal omdat de backend-service is beveiligd met Azure AD-verificatie. Dit is geïllustreerd in de video beginnend bij 7:40 met de volgende stappen.

Klik op **Developer portal** vanaf de boven-rechterkant van de portal van publisher.

![Developer portal][api-management-developer-portal-menu]

**API's** en klik op de **Rekenmachine** API.

![Developer portal][api-management-dev-portal-apis]

Klik op **het uitproberen**.

![Het uitproberen][api-management-dev-portal-try-it]

Klik op **verzenden** en noteer de antwoordstatus van **401 niet gemachtigd**.

![Verzenden][api-management-dev-portal-send-401]

Het verzoek is niet geautoriseerd omdat de backend-API is beveiligd met Azure Active Directory. Voordat u kunt de API de ontwikkelaar is portal als u akkoord gaat met OAuth 2.0 ontwikkelaars geconfigureerd. Dit proces wordt beschreven in de volgende secties.

## <a name="register-the-developer-portal-as-an-aad-application"></a>De developer portal registreren als een AAD-toepassing

De eerste stap bij het configureren van de developer portal als u akkoord gaat met OAuth 2.0 ontwikkelaars is de developer portal registreren als een AAD-toepassing. Dit wordt geïllustreerd beginnend bij 8:27 in de video.

Navigeer naar de Azure AD-tenant van de eerste stap van deze video, klikt u in dit voorbeeld **APIMDemo** en Ga naar het tabblad **toepassingen** .

![Nieuwe toepassing][api-management-aad-new-application-devportal]

Klik op de knop **toevoegen** om een nieuwe Azure Active Directory-toepassing te maken en kies van **toepassing is bij het ontwikkelen van mijn organisatie toevoegen**.

![Nieuwe toepassing][api-management-new-aad-application-menu]

Kies **webtoepassing en/of Web API**, voer een naam en klik op de pijl-rechts. In dit voorbeeld wordt **APIMDeveloperPortal** gebruikt.

![Nieuwe toepassing][api-management-aad-new-application-devportal-1]

Voer de URL van uw service API Management en toevoegen voor **eenmalige aanmelding URL** `/signin`. In dit voorbeeld wordt **https://contoso5.portal.azure-api.net/signin **gebruikt.

Voer de URL van uw API Management-service voor **URL van de App-Id** en enkele unieke tekens toevoegen. Deze kunnen worden gewenste tekens en in dit voorbeeld **https://contoso5.portal.azure-api.net/dp** wordt gebruikt. Wanneer de gewenste **App eigenschappen** zijn geconfigureerd, klikt u op het vinkje om de toepassing te maken.

![Nieuwe toepassing][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a>Een API Management OAuth 2.0 autorisatie configureren

De volgende stap is een OAuth 2.0 autorisatie-server API beheer ter configureren. Deze stap wordt geïllustreerd in de video beginnen bij 9:43.

Klik op **beveiliging** in het menu API beheer aan de linkerkant op **OAuth 2.0**en klik vervolgens op **toevoegen autorisatie** -server.

![Autorisatie voor server toevoegen][api-management-add-authorization-server]

Geef een naam en een optionele beschrijving in de velden **naam** en **Beschrijving** . Deze velden worden gebruikt om aan te geven van de server OAuth 2.0 autorisatie binnen het exemplaar van de service API Management. In dit voorbeeld wordt **autorisatie server demo** gebruikt. Later wanneer u een OAuth 2.0-server moet worden gebruikt voor verificatie in voor een API opgeeft, selecteert u deze naam.

Voor de **URL voor de aanmeldingspagina van Client registratie** voert u een tijdelijke aanduiding voor waarde, zoals `http://localhost`.  De **URL voor de aanmeldingspagina van Client registratie** verwijst naar de pagina waarmee gebruikers kunnen maken en configureren van hun eigen accounts voor OAuth 2.0-providers die ondersteuning bieden voor gebruikersbeheer accounts. Gebruikers niet in dit voorbeeld maken en configureren van hun eigen accounts, zodat een tijdelijke aanduiding voor wordt gebruikt.

![Autorisatie voor server toevoegen][api-management-add-authorization-server-1]

Geef vervolgens **autorisatie eindpunt URL** en **Token eindpunt-URL**.

![Autorisatie server][api-management-add-authorization-server-1a]

Deze waarden kunnen uit de **Eindpunten van de App** -pagina van de AAD-toepassing die u hebt gemaakt voor de developer portal worden opgehaald. Voor toegang tot de eindpunten Ga naar het tabblad **configureren** voor de AAD-toepassing en klik op **weergave-eindpunten**.

![Toepassing][api-management-aad-devportal-application]

![Weergave eindpunten][api-management-aad-view-endpoints]

Het **OAuth 2.0 autorisatie eindpunt** Kopieer en plak deze in het tekstvak **URL-autorisatie eindpunt** .

![Autorisatie voor server toevoegen][api-management-add-authorization-server-2]

Het **OAuth 2.0 token eindpunt** Kopieer en plak deze in het tekstvak **URL voor Token-eindpunt** .

![Autorisatie server toevoegen][api-management-add-authorization-server-2a]

Naast plakken in het token eindpunt, moet u een parameter extra hoofdtekst benoemde **resource** en voor de waarde-gebruik de **App-Id-URI** uit de AAD-toepassing voor de backend-service die is gemaakt wanneer het project Visual Studio is gepubliceerd toevoegen.

![App-Id URI][api-management-aad-sso-uri]

Geef vervolgens de referenties van de client. Hierna ziet u de referenties voor de resource die u wilt openen, in dit geval de backend-service.

![Clientreferenties][api-management-client-credentials]

Als u de **Client-Id**, Ga naar het tabblad **configureren** van de AAD-toepassing voor de backend-service en kopieert u de **Client-Id**.

Voor het ophalen **Geheim Client** klik vervolgens op de **duur Selecteer** omlaag in de sectie **toetsen** en een interval opgeven. In dit voorbeeld wordt 1 jaar gebruikt.

![Cliënt-ID][api-management-aad-client-id]

Klik op **Opslaan** om de configuratie opslaan en de toets weer te geven. 

>[AZURE.IMPORTANT] Noteer deze toets. Wanneer u het venster van de configuratie Azure Active Directory hebt gesloten, kan de toets niet opnieuw worden weergegeven.

De toets naar het Klembord kopiëren, gaat u terug naar de publisher-portal, de toets in het tekstvak **Geheim Client** plakken en klik op **Opslaan**.

![Autorisatie server toevoegen][api-management-add-authorization-server-3]

Direct na de referenties van de client is een autorisatie code verlenen. Kopieer deze autorisatiecode en schakelt u terug naar uw Azure AD developer portal-toepassing configureren pagina en plakken de machtiging verlenen in het veld **Antwoord URL** en klik nogmaals op **Opslaan** .

![Antwoord URL][api-management-aad-reply-url]

De volgende stap is voor het configureren van de machtigingen voor de developer portal AAD-toepassing. Klik op **Machtigingen** en schakel het selectievakje voor **directory-gegevens lezen**. Klik op **Opslaan** om het opslaan van deze wijziging en klik vervolgens op **toepassing toevoegen**.

![Machtigingen toevoegen][api-management-add-devportal-permissions]

Klik op het zoekpictogram, typ **APIM** in het vak vanaf **APIMAADDemo**selecteren en klik op het vinkje om op te slaan.

![Machtigingen toevoegen][api-management-aad-add-app-permissions]

Klik op **Gedelegeerde machtigingen** voor **APIMAADDemo** en schakel het selectievakje voor **Access APIMAADDemo**en op **Opslaan**. In deze sectie geeft de ontwikkelaar portal toepassing voor toegang tot de backend-service.

![Machtigingen toevoegen][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-the-calculator-api"></a>OAuth 2.0 gebruikersautorisatie voor de API Rekenmachine inschakelen

Nu dat het OAuth 2.0-server is geconfigureerd, kunt u deze in de beveiligingsinstellingen voor uw API opgeeft. Deze stap wordt geïllustreerd in de video vanaf 14:30.

**API's** in het linkermenu op en klik op **Rekenmachine** de instellingen wilt bekijken en te configureren.

![Rekenmachine API][api-management-calc-api]

Ga naar het tabblad **beveiliging** , schakel het selectievakje **OAuth 2.0** , selecteer de gewenste autorisatie-server in de vervolgkeuzelijst **autorisatie server** en klik op **Opslaan**.

![Rekenmachine API][api-management-enable-aad-calculator]

## <a name="successfully-call-the-calculator-api-from-the-developer-portal"></a>Is de API Rekenmachine bellen vanuit de developer portal

Nu dat het OAuth 2.0-autorisatie is geconfigureerd op de API, kunnen de bewerkingen goed worden aangeroepen vanuit het developer center. Deze stap wordt geïllustreerd in de video beginnen bij 15:00.

Ga terug naar de bewerking **toevoegen twee gehele getallen** van de service Rekenmachine in de portal voor ontwikkelaars en klik op **het uitproberen**. Houd rekening met het nieuwe item in de sectie **autorisatie** voor de autorisatie-server die u zojuist hebt toegevoegd.

![Rekenmachine API][api-management-calc-authorization-server]

Selecteer **autorisatiecode** in de vervolgkeuzelijst autorisatie en voer de referenties van het account te gebruiken. Als u al zijn aangemeld met het account dat u mogelijk niet gevraagd.

![Rekenmachine API][api-management-devportal-authorization-code]

Klik op **verzenden** en noteer de **antwoordstatus** van **200 OK** en de resultaten van de bewerking in de inhoud van de reactie.

![Rekenmachine API][api-management-devportal-response]

## <a name="configure-a-desktop-application-to-call-the-api"></a>Een bureaubladtoepassing de API aan te roepen configureren

De volgende procedure in de video begint bij 16:30 en configureert een eenvoudige bureaubladtoepassing de API aan te roepen. De eerste stap is de bureaubladtoepassing registreren in Azure AD en toegang geven tot de adreslijst en naar de backend-service. 18:25 is er een demonstratie van de bellen van een bewerking op de Rekenmachine API-bureaubladtoepassing hebt.

## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>Een beleid voor JWT validatie vooraf om aanvragen te verifiëren configureren

De uiteindelijke procedure in de video begint bij 20:48 en ziet u hoe u met het beleid [JWT valideren](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) vooraf Autoriseer aanvragen door het valideren van de access-tokens van elke binnenkomende aanvraag kunt invullen. Als het verzoek niet worden gevalideerd door het beleid JWT valideren, wordt de aanvraag is geblokkeerd door de API Management en is niet doorgegeven aan de backend.

    <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
        <openid-config url="https://login.windows.net/DemoAPIM.onmicrosoft.com/.well-known/openid-configuration" />
        <required-claims>
            <claim name="aud">
                <value>https://DemoAPIM.NOTonmicrosoft.com/APIMAADDemo</value>
            </claim>
        </required-claims>
    </validate-jwt>

Zie voor een andere demonstratie van configureren en gebruiken van dit beleid [Cloud begeleidende aflevering 177: meer API beheerfuncties](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) en vooruitspoelen op 13:50. Vooruitspoelen naar 15:00 om te zien van het beleid is geconfigureerd in de editor en vervolgens naar 18:50 voor een demonstratie van het aanroepen van een bewerking van de developer portal zowel met en zonder het token vereiste verificatie.

## <a name="next-steps"></a>Volgende stappen
-   Meer [video's](https://azure.microsoft.com/documentation/videos/index/?services=api-management) over API beheertaken uitchecken.
-   Zie [onderlinge certificaatverificatie](api-management-howto-mutual-certificates.md) en [verbinding maken via een VPN- of ExpressRoute](api-management-howto-setup-vpn.md)voor andere manieren om te beveiligen van uw backend-service.

[api-management-management-console]: ./media/api-management-howto-protect-backend-with-aad/api-management-management-console.png

[api-management-import-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-new-api.png
[api-management-create-aad-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad-menu.png
[api-management-create-aad]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad.png
[api-management-new-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-web-app.png
[api-management-new-project]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project.png
[api-management-new-project-cloud]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project-cloud.png
[api-management-change-authentication]: ./media/api-management-howto-protect-backend-with-aad/api-management-change-authentication.png
[api-management-sign-in-vidual-studio]: ./media/api-management-howto-protect-backend-with-aad/api-management-sign-in-vidual-studio.png
[api-management-configure-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-configure-web-app.png
[api-management-aad-domains]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-domains.png
[api-management-add-controller]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-controller.png
[api-management-web-publish]: ./media/api-management-howto-protect-backend-with-aad/api-management-web-publish.png
[api-management-aad-backend-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-backend-app.png
[api-management-aad-add-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-permissions.png
[api-management-developer-portal-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-developer-portal-menu.png
[api-management-dev-portal-apis]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-apis.png
[api-management-dev-portal-try-it]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-try-it.png
[api-management-dev-portal-send-401]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-send-401.png
[api-management-aad-new-application-devportal]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal.png
[api-management-aad-new-application-devportal-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-1.png
[api-management-aad-new-application-devportal-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-2.png
[api-management-aad-devportal-application]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-devportal-application.png
[api-management-add-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server.png
[api-management-aad-sso-uri]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-sso-uri.png
[api-management-aad-view-endpoints]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-view-endpoints.png
[api-management-aad-client-id]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-client-id.png
[api-management-add-authorization-server-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1.png
[api-management-add-authorization-server-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2.png
[api-management-add-authorization-server-2a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2a.png
[api-management-add-authorization-server-3]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-3.png
[api-management-aad-reply-url]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-reply-url.png
[api-management-add-devportal-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-devportal-permissions.png
[api-management-aad-add-app-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-app-permissions.png
[api-management-aad-add-delegated-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-delegated-permissions.png
[api-management-calc-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-api.png
[api-management-enable-aad-calculator]: ./media/api-management-howto-protect-backend-with-aad/api-management-enable-aad-calculator.png
[api-management-devportal-authorization-code]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-authorization-code.png
[api-management-devportal-response]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-response.png
[api-management-calc-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-authorization-server.png
[api-management-add-authorization-server-1a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1a.png
[api-management-client-credentials]: ./media/api-management-howto-protect-backend-with-aad/api-management-client-credentials.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-aad-application-menu.png

[Een exemplaar van de service API Management maken]: api-management-get-started.md#create-service-instance
[Uw eerste API beheren]: api-management-get-started.md
