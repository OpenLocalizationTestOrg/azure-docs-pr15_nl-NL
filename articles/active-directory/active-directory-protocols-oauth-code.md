<properties
    pageTitle="Azure AD .NET Protocol overzicht | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe u toegang tot webtoepassingen en web API's in de tenant met behulp van de Azure Active Directory en OAuth 2.0 Autoriseer met HTTP-berichten."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>


# <a name="authorize-access-to-web-applications-using-oauth-20-and-azure-active-directory"></a>Autoriseer toegang tot webtoepassingen met OAuth 2.0 en Azure Active Directory

Azure Active Directory (Azure AD) gebruik OAuth 2.0 zodat u kunt de toegang tot webtoepassingen en web API's in uw Azure AD-tenant. Deze handleiding is taal onafhankelijke en wordt beschreven hoe u berichten verzenden en ontvangen HTTP zonder een van onze open source-bibliotheken.

De stroom van OAuth 2.0 autorisatie code wordt beschreven in de [sectie 4.1 van het OAuth 2.0-specificatie](https://tools.ietf.org/html/rfc6749#section-4.1) . Deze wordt gebruikt voor verificatie en machtiging uitvoeren in de meeste typen app, met inbegrip van de WebApps en apps standaard geïnstalleerd.

[AZURE.INCLUDE [active-directory-protocols-getting-started](../../includes/active-directory-protocols-getting-started.md)]


## <a name="oauth-20-authorization-flow"></a>OAuth 2.0 autorisatie stroom

Op hoog niveau, de hele autorisatie stroom voor een toepassing voor iets zien er zo uit:

![OAuth Auth Code stroom](media/active-directory-protocols-oauth-code/active-directory-oauth-code-flow-native-app.png)


## <a name="request-an-authorization-code"></a>Een autorisatiecode aanvragen

De stroom van de code autorisatie begint met de client die de gebruiker waarmee de `/authorize` eindpunt. In dit verzoek geeft de client aan de machtigingen die deze moeten ophalen van de gebruiker. U kunt de eindpunten OAuth 2.0 krijgen van van uw toepassing pagina in klassieke Azure-Portal, de knop **Weergave eindpunten** in de onder-vel in.

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&resource=https%3A%2F%2Fservice.contoso.com%2F
&state=12345
```

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | --------------- |
| tenant | Vereist | De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie kan Meld u aan bij de toepassing.  De toegestane waarden zijn tenant-id's, bijvoorbeeld `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` of `contoso.onmicrosoft.com` of `common` voor de tenant-onafhankelijke tokens |
| client_id | Vereist | De toepassings-Id toegewezen aan uw app wanneer u deze hebt geregistreerd bij Azure AD. U kunt dit vinden in de beheerportal Azure. Klik op **Active Directory**, klikt u op de map, klikt u op de toepassing en klik vervolgens op in **configureren** |
| response_type | Vereist | Moet bevatten `code` voor de stroom van de code autorisatie. |
| redirect_uri | aanbevolen | De redirect_uri van de app, waar verificatie antwoorden kunnen worden verzonden en ontvangen door de app.  Dit moet precies overeenkomen met een van de redirect_uris die u in de portal geregistreerd, behalve het url-codering moet zijn.  Voor systeemeigen en mobiele apps, moet u de standaardwaarde van `urn:ietf:wg:oauth:2.0:oob`. |
| response_mode | aanbevolen | Hiermee geeft u de methode die moet worden gebruikt voor het verzenden van de resulterende token terug naar uw app.  Kan `query` of `form_post`.  |
| de staat | aanbevolen | Een waarde die is opgenomen in de aanvraag dat ook in het token antwoord wordt geretourneerd. Een willekeurig wordt gegenereerd unieke waarde wordt meestal gebruikt voor [voorkoming van meerdere sites verzoek aanvallen](http://tools.ietf.org/html/rfc6749#section-10.12).  De staat, wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek is opgetreden, bijvoorbeeld een pagina of weergave matrixgegevens op. |
| resource | optionele | De App-ID-URI van het web API (beveiligde resource). Als u de App-ID-URI van het web API, zoekt in de beheerportal Azure, klik op **Active Directory**, klikt u op de map, klik op de toepassing en klik vervolgens op **configureren**. |
| prompt | optionele |  Geef het type van de interactie van de gebruiker die is vereist.<p> Geldige waarden zijn: <p> *Login*: de gebruiker moet worden gevraagd opnieuw worden geverifieerd. <p> *toestemming*: toestemming van de gebruiker is verleend, maar moet worden bijgewerkt. De gebruiker moet worden gevraagd toe te staan. <p> *admin_consent*: een beheerder moet worden gevraagd naar toestemming namens alle gebruikers in hun organisatie |
| login_hint | optionele | Kan worden gebruikt om te vullen vooraf het veld gebruikersnaam/e-mail adres van de aanmeldingspagina voor de gebruiker, als u weet dat hun gebruikersnaam tijd vooraf.  Vaak apps deze parameter wordt gebruikt tijdens opnieuw worden geverifieerd, de gebruikersnaam die al worden opgehaald uit een vorige aanmelden met de `preferred_username` claimen. |
| domain_hint | optionele | Biedt een tip over de tenant of het domein dat de gebruiker gebruiken moet voor het aanmelden. De waarde van de domain_hint is een geregistreerde domein voor de tenant. Als de tenant is gekoppeld aan een on-premises adreslijst, wordt AAD omgeleid naar de federatieserver opgegeven tenant. |

> [AZURE.NOTE] Als de gebruiker deel van een organisatie uitmaakt, kan een beheerder van de organisatie toestemming of weigeren van de gebruiker namens of dat de gebruiker toe te staan. De optie voor het alleen als de beheerder toestemming geeft deze dat toestemming krijgt de gebruiker.

Nu de gebruiker wordt gevraagd hun referenties invoeren en akkoord met de machtigingen zoals aangegeven in de `scope` queryparameter. Nadat de gebruiker wordt geverifieerd, en toestemming verleent, Azure AD stuurt een antwoord naar uw app in de `redirect_uri` adres in uw aanvraag kunt invullen.

### <a name="successful-response"></a>Positief antwoord

Een positief antwoord kan er als volgt uit:

```
GET  HTTP/1.1 302 Found
Location: http://localhost/myapp/?code= AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA&session_state=7B29111D-C220-4263-99AB-6F6E135D75EF&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| Parameter | Beschrijving |
| -----------------------| --------------- |
| admin_consent | De waarde is True als een beheerder ingestemd met een vraag toestemming om.|
| code | De autorisatiecode die de toepassing wordt gevraagd. De toepassing kan de autorisatiecode aanvragen van een toegangstoken voor de resource doel gebruiken. |
| session_state | Een unieke waarde die aangeeft van de huidige gebruikerssessie van de. Deze waarde is een GUID, maar moet worden behandeld als een ondoorzichtig waarde die wordt doorgegeven zonder onderzoek. |
| de staat | Als een parameter status is opgenomen in de aanvraag hetzelfde resultaat moet worden weergegeven in de reactie. Het is een goede gewoonte voor de toepassing om te bevestigen dat de waarden staat in de vergaderverzoeken en antwoorden identiek zijn voordat u het antwoord wordt gebruikt. Hiermee kunt u [meerdere sites aanvragen voorkoming (CSRF)-aanvallen](https://tools.ietf.org/html/rfc6749#section-10.12) detecteren ten opzichte van de client.  

### <a name="error-response"></a>Foutmelding

Antwoorden van de fout kunnen ook worden verzonden naar de `redirect_uri` zodat de toepassing deze correct kan verwerken.

```
GET http://localhost:12345/?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschrijving |
|-----------|-------------|
| Fout | De waarde van een fout code is gedefinieerd in de sectie 5,2 van het [OAuth 2.0 autorisatie Framework](http://tools.ietf.org/html/rfc6749). De volgende tabel beschrijft de foutcodes die Azure AD geeft als resultaat. |
| error_description | Een gedetailleerde beschrijving van de fout. Dit bericht is niet bedoeld om te worden eindgebruikers vriendelijke. |
| de staat | De waarde status is een willekeurig wordt gegenereerd, niet opnieuw gebruikt waarde die is verzonden in de uitnodiging en geretourneerd door het antwoord op meerdere sites verzoek voorkoming (CSRF) aanvallen te voorkomen. |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Foutcodes autorisatie eindpunt fouten

De volgende tabel beschrijft de verschillende foutcodes die kunnen worden geretourneerd in de `error` -parameter van de reactie van de fout.

| Foutcode | Beschrijving | Clientactie |
|------------|-------------|---------------|
| invalid_request | Protocolfout, zoals een ontbrekende vereiste parameter. | Oplossen en verzend het verzoek opnieuw. Dit is een ontwikkeling fout wordt meestal onderschept tijdens de eerste test.|
| unauthorized_client | De clienttoepassing is niet toegestaan aanvragen van een autorisatiecode. | Dit gebeurt meestal wanneer de clienttoepassing is niet geregistreerd in Azure AD of niet wordt toegevoegd aan van de gebruiker Azure AD-tenant. De toepassing kan de gebruiker met instructies voor het installeren van de toepassing en toevoegen aan Azure AD gevraagd. |
| ACCESS_DENIED | De eigenaar van de resource is geweigerd toestemming | De clienttoepassing kan de gebruiker die dit kan niet worden voortgezet tenzij de gebruiker akkoord gaat melden. |
| unsupported_response_type | De server autorisatie biedt geen ondersteuning voor het antwoordtype in het verzoek. | Oplossen en verzend het verzoek opnieuw. Dit is een ontwikkeling fout wordt meestal onderschept tijdens de eerste test.|
|server_error | De server is een onverwachte fout opgetreden. | Probeer het verzoek. Deze fouten kunnen resulteren uit tijdelijke voorwaarden. De clienttoepassing mogelijk wordt uitgelegd aan de gebruiker die het antwoord is vertraagd vanwege een tijdelijke fout. |
| temporarily_unavailable | De server is tijdelijk bezet u omgaat met het verzoek. | Probeer het verzoek. De clienttoepassing mogelijk wordt uitgelegd aan de gebruiker dat het antwoord is vertraagd vanwege een tijdelijke voorwaarde. |
| invalid_resource |De doel-resource is ongeldig omdat deze niet bestaat, Azure AD kan deze niet vinden of deze juist niet is geconfigureerd.| Hiermee geeft u aan dat de resource, indien aanwezig, niet is geconfigureerd in de tenant. De toepassing kan de gebruiker met instructies voor het installeren van de toepassing en toevoegen aan Azure AD gevraagd. |

## <a name="use-the-authorization-code-to-request-an-access-token"></a>Gebruik de autorisatiecode voor een toegangstoken aanvragen

Nu dat u hebt aangeschaft van een autorisatiecode en machtigingen hebt gekregen van de gebruiker, kunt u de code voor een toegangstoken naar de gewenste resource inwisselen door te sturen van een POST-aanvraag naar de `/token` eindpunt:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code
&client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA
&redirect_uri=https%3A%2F%2Flocalhost%2Fmyapp%2F
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=p@ssw0rd

//NOTE: client_secret only required for web apps
```

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | --------------------- |
| tenant | Vereist |  De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie kan Meld u aan bij de toepassing.  De toegestane waarden zijn tenant-id's, bijvoorbeeld `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` of `contoso.onmicrosoft.com` of `common` voor de tenant-onafhankelijke tokens |
| client_id | Vereist | De toepassings-Id toegewezen aan uw app wanneer u deze hebt geregistreerd bij Azure AD. U kunt dit vinden in de klassieke Azure-Portal. Klik op **Active Directory**, klikt u op de map, klikt u op de toepassing en klik vervolgens op in **configureren** |
| grant_type | Vereist | Moet `authorization_code` voor de stroom van de code autorisatie. |
| code | Vereist | De `authorization_code` die u in de vorige sectie hebt aangeschaft   |
| redirect_uri | Vereist | Hetzelfde `redirect_uri` waarde die is gebruikt om de `authorization_code`. |
| client_secret | vereist voor WebApps | De toepassing geheim die u in de portal van de registratie van de app hebt gemaakt voor de app.  Deze moet niet worden gebruikt in een systeemeigen app, omdat client_secrets betrouwbaar op apparaten kan niet worden opgeslagen.  Dit is vereist voor het WebApps en web API's waarvoor de mogelijkheid om op te slaan de `client_secret` veilig op de server. |
| resource | Als het opgegeven in autorisatie code aanvraag, anders optioneel vereist | De App-ID-URI van het web API (beveiligde resource).
Als u de App-ID-URI, in de beheerportal Azure zoekt, klikt u op **Active Directory**, klikt u op de map, klik op de toepassing en klik vervolgens op **configureren**.

### <a name="successful-response"></a>Positief antwoord

Azure AD geeft als resultaat een toegangstoken na een geslaagde reactie. Als u wilt minimaliseren netwerk oproepen van de clienttoepassing en hun bijbehorende latentie, moet de clienttoepassing access tokens cache voor de levensduur van tokens die is opgegeven in het OAuth 2.0-antwoord. Gebruik om te bepalen de levensduur van tokens, hetzij de `expires_in` of `expires_on` parameterwaarden.

Als een web API resource geeft als resultaat een `invalid_token` foutcode, dit kan betekenen dat de resource heeft vastgesteld dat het token is verlopen. Als de client- en resourcekalenders klok tijden zijn verschillende (ook wel een "scheefheid time"), is de resource overwegen de token verlopen voordat het token uit de clientcache is uitgeschakeld. Als dit gebeurt, schakelt u het token uit de cache, zelfs als deze zich nog steeds in de berekende levensduur.

Een positief antwoord kan er als volgt uit:

```
{
  "access_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1388444763",
  "resource": "https://service.contoso.com/",
  "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA",
  "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
"id_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.”
}

```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| access_token | De gevraagde toegangstoken. De app kunt u deze token gebruiken om de beveiligde resource, zoals een web API te verifiëren. |
| token_type | Geeft de waarde type token. Het enige type die ondersteuning biedt voor Azure AD is vruchtdragende. Zie voor meer informatie over vruchtdragende tokens, [OAuth2.0 autorisatie Framework: vruchtdragende Token gebruik (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt)  |
| expires_in | Hoe lang het toegangstoken geldt (in seconden). |
| expires_on | Het tijdstip waarop het toegangstoken verloopt. De datum wordt weergegeven als het aantal seconden vanaf 1970-01-01T0:0:0Z UTC totdat de verlooptijd. Deze waarde wordt gebruikt om te bepalen de levensduur van in de cache tokens. |
| resource | De App-ID-URI van het web API (beveiligde resource).|
| bereik | Imitatie machtigingen met de clienttoepassing. Standaard de machtiging is `user_impersonation`. De eigenaar van de beveiligde bron kunt extra waarden registreren in Azure AD.|
| refresh_token |  Een OAuth 2.0 vernieuwen token. De app met deze token kunt u in het bezit van extra access tokens als het huidige toegangstoken is verlopen.  Vernieuwen tokens zijn lange levensduur en kunnen worden gebruikt om toegang tot bronnen voor langere tijd behouden. |
| id_token | Een niet-ondertekende JSON Web Token (JWT). De app kunnen base64Url decoderen de segmenten van deze token informatie opvragen over de gebruiker die heeft aangemeld. De app kan de waarden in de cache en waar ze worden weergegeven, maar deze mag niet volledig vertrouwen worden voor alle autorisatie en beveiligingsgrenzen. |

### <a name="jwt-token-claims"></a>JWT Token Claims
De token JWT in de waarde van de `id_token` parameter kan worden verkregen in de volgende argumenten:

```
{
 "typ": "JWT",
 "alg": "none"
}.
{
 "aud": "2d4d11a2-f814-46a7-890a-274a72a7309e",
 "iss": "https://sts.windows.net/7fe81447-da57-4385-becb-6de57f21477e/",
 "iat": 1388440863,
 "nbf": 1388440863,
 "exp": 1388444763,
 "ver": "1.0",
 "tid": "7fe81447-da57-4385-becb-6de57f21477e",
 "oid": "68389ae2-62fa-4b18-91fe-53dd109d74f5",
 "upn": "frank@contoso.com",
 "unique_name": "frank@contoso.com",
 "sub": "JWvYdCWPhhlpS1Zsf7yYUxShUwtUm5yzPmw_-jX3fHY",
 "family_name": "Miller",
 "given_name": "Frank"
}.
```

De `id_token` parameter bevat de volgende typen. Voor meer informatie over JSON web tokens, raadpleegt u de [JWT IETF conceptspecificatie](http://go.microsoft.com/fwlink/?LinkId=392344). Voor meer informatie over de token typen en claims Lees [Token ondersteund en typen](active-directory-token-and-claims.md)

| Type claimen | Beschrijving |
|------------|-------------|
| AUD | Een doelgroep van het token. Wanneer het token is uitgegeven naar een clienttoepassing, is dat het publiek de `client_id` van de client.
| EXP | Verlooptijd. Het tijdstip waarop het token verloopt. Voor het token geldig, de huidige datum/tijd moet kleiner zijn dan of gelijk aan de `exp` waarde. De tijd wordt weergegeven als het aantal seconden van 1 januari 1970 (1970-01-01T0:0:0Z) UTC tot op het moment dat de token is uitgegeven. |
| family_name | Gebruikerswachtwoord achternaam of achternaam. De toepassing kunt deze waarde weergeven. |
| given_name | Voornaam van gebruiker. De toepassing kunt deze waarde weergeven. |
| IAT | Uitgegeven tegelijk. De tijd waarop de JWT is verstrekt. De tijd wordt weergegeven als het aantal seconden van 1 januari 1970 (1970-01-01T0:0:0Z) UTC tot op het moment dat de token is uitgegeven. |
| ISS | Geeft aan wat het token uitgever |
| NBF | Niet eerder tijd. Het tijdstip waarop het token van kracht. Voor het token geldig, moet de huidige datum/tijd groter dan of gelijk is aan de waarde Nbf. De tijd wordt weergegeven als het aantal seconden van 1 januari 1970 (1970-01-01T0:0:0Z) UTC tot op het moment dat de token is uitgegeven. |
| OID | Object-id (ID) van het gebruikersobject in Azure AD. |
| Sub | Token onderwerp-id. Dit is een permanente en onveranderlijke id voor de gebruiker waarmee het token beschreven. Gebruik deze waarde in cache opslaan logica. |
| TID | Tenant-id (ID) van de Azure AD-tenant dat het token uitgegeven. |
| unique_name | Een unieke id voor die kan worden weergegeven voor de gebruiker. Dit is meestal een UPN (User Principal Name). |
| UPN | UPN van de gebruiker. |
| ver | Versie. De versie van het JWT token, meestal 1.0. |

### <a name="error-response"></a>Foutmelding

De token uitgifte eindpunt fouten zijn HTTP-foutcodes, omdat de client het eindpunt token uitgifte rechtstreeks belt. Naast de HTTP-statuscode resulteert het eindpunt van de token uitgifte Azure AD ook in een document JSON met objecten die de fout te beschrijven.

Een antwoord van de fout steekproef kan er als volgt uit:

```
{
  "error": "invalid_grant",
  "error_description": "AADSTS70002: Error validating credentials. AADSTS70008: The provided authorization code or refresh token is expired. Send a new interactive authorization request for this user and resource.\r\nTrace ID: 3939d04c-d7ba-42bf-9cb7-1e5854cdce9e\r\nCorrelation ID: a8125194-2dc8-4078-90ba-7b6592a7f231\r\nTimestamp: 2016-04-11 18:00:12Z",
  "error_codes": [
    70002,
    70008
  ],
  "timestamp": "2016-04-11 18:00:12Z",
  "trace_id": "3939d04c-d7ba-42bf-9cb7-1e5854cdce9e",
  "correlation_id": "a8125194-2dc8-4078-90ba-7b6592a7f231"
}
```
| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt om de soorten fouten die optreden classificeren en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar kunt identificeren de oorzaak van een verificatiefout.  |
| error_codes | Een lijst met STS specifieke foutcodes die in diagnostische hulpprogramma's kunnen helpen. |
| tijdstempel | De tijd waarop de fout is opgetreden. |
| trace_id | Een unieke id voor de aanvraag die u kunt bij het oplossen van diagnostische gegevens.  |
| correlation_id | Een unieke id voor de aanvraag die u kunt bij het oplossen van diagnostische gegevens over de onderdelen.|

#### <a name="http-status-codes"></a>HTTP-statuscodes

De volgende tabel bevat de HTTP-statuscodes die u kunt het eindpunt token uitgifte geeft als resultaat. In sommige gevallen, de foutcode is voldoende om te beschrijven van het antwoord, maar in geval van fouten, moet u parseren van het bijbehorende JSON-document en de foutcode te onderzoeken.

| HTTP-Code | Beschrijving |
|-----------|-------------|
| 400       | Standaard HTTP-code. In de meeste gevallen gebruikt en is meestal vanwege een ongeldige aanvraag. Oplossen en verzend het verzoek opnieuw. |
| 401       | Verificatie is mislukt. De aanvraag ontbreekt bijvoorbeeld de parameter client_secret.|
| 403       | Verificatie mislukt. Bijvoorbeeld: de gebruiker geen toegangsrechten voor de bron. |
| 500       | Een interne fout opgetreden bij de service. Probeer het verzoek. |

#### <a name="error-codes-for-token-endpoint-errors"></a>Foutcodes voor token-eindpunt fouten

| Foutcode | Beschrijving | Clientactie |
|------------|-------------|---------------|
| invalid_request | Protocolfout, zoals een ontbrekende vereiste parameter. | Los en de aanvraag opnieuw indienen |
| invalid_grant | De autorisatiecode ongeldig is of is verlopen. | Voer een nieuwe aanvraag naar de `/authorize` eindpunt |
| unauthorized_client | De geverifieerde client is niet geautoriseerd gebruik van dit type met toestemming verlenen. | Dit gebeurt meestal wanneer de clienttoepassing is niet geregistreerd in Azure AD of niet wordt toegevoegd aan van de gebruiker Azure AD-tenant. De toepassing kan de gebruiker met instructies voor het installeren van de toepassing en toevoegen aan Azure AD gevraagd. |
| invalid_client | Clientverificatie is mislukt. | De referenties van de client zijn niet geldig. De beheerder van de toepassing bijgewerkt om op te lossen, de referenties. |
| unsupported_grant_type | De server autorisatie biedt geen ondersteuning voor het type van toestemming verlenen. | Wijzig het type verlenen in het verzoek. Dit soort fouten alleen tijdens de ontwikkeling moet worden uitgevoerd en worden gedetecteerd tijdens de eerste test. |
| invalid_resource | De doel-resource is ongeldig omdat deze niet bestaat, Azure AD kan deze niet vinden of deze juist niet is geconfigureerd. | Hiermee geeft u aan dat de resource, indien aanwezig, niet is geconfigureerd in de tenant. De toepassing kan de gebruiker met instructies voor het installeren van de toepassing en toevoegen aan Azure AD gevraagd. |
| interaction_required | Het verzoek is vereist voor interactie met de gebruiker. Bijvoorbeeld, is een stap extra verificatie vereist. | Probeer het verzoek met dezelfde resource. |
| temporarily_unavailable | De server is tijdelijk bezet u omgaat met het verzoek. | Probeer het verzoek. De clienttoepassing mogelijk wordt uitgelegd aan de gebruiker dat het antwoord is vertraagd vanwege een tijdelijke voorwaarde.|

## <a name="use-the-access-token-to-access-the-resource"></a>Het toegangstoken gebruiken voor toegang tot de resource

Nu dat u hebt gekregen een `access_token`, kunt u het token in aanvragen voor Web API's, door op te nemen in de `Authorization` koptekst. De specificatie [RFC 6750](http://www.rfc-editor.org/rfc/rfc6750.txt) wordt uitgelegd hoe u vruchtdragende tokens in HTTP-aanvragen voor toegang tot beveiligde bronnen.

### <a name="sample-request"></a>Voorbeeld van een aanvraag

```
GET /data HTTP/1.1
Host: service.contoso.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ
```

### <a name="error-response"></a>Foutmelding

Beveiligde bronnen en implementeren van RFC 6750 probleem HTTP-statuscodes. Als het verzoek niet bij verificatiereferenties inbegrepen is of het token ontbreekt, het antwoord bevat een `WWW-Authenticate` koptekst. Wanneer een nieuw vergaderverzoek mislukt, reageert de resource-server met de een HTTP-statuscode en een foutcode.

Hier volgt een voorbeeld van een mislukt antwoord wordt verzonden wanneer de clientaanvraag niet bij de dragertoken inbegrepen is:

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.window.net/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="The access token is missing.",
```

#### <a name="error-parameters"></a>Foutparameters

| Parameter | Beschrijving |
|-----------|-------------|
| authorization_uri | De URI (fysieke eindpunt) van de server autorisatie. Deze waarde wordt ook gebruikt als de sleutel van een zoekactie voor meer informatie over de server uit een discovery-eindpunt. <p><p> De client moet controleren of de server autorisatie vertrouwde. Wanneer de resource is beveiligd met Azure AD, hoeft u voldoende om te bevestigen dat de URL begint met https://login.windows.net of een ander hostname die ondersteuning biedt voor Azure AD. Een resource tenant / regiospecifieke moet altijd een tenant-specifieke autorisatie URI retourneren. |
| Fout | De waarde van een fout code is gedefinieerd in de sectie 5,2 van het [OAuth 2.0 autorisatie Framework](http://tools.ietf.org/html/rfc6749).|
| error_description | Een gedetailleerde beschrijving van de fout. Dit bericht is niet bedoeld om te worden eindgebruikers vriendelijke.|
| bron-id | Geeft als resultaat de unieke id van de resource. De clienttoepassing deze id kunt gebruiken als de waarde van de `resource` parameter na het aanvragen van een token voor de resource. <p><p> Is het belangrijk is voor de clienttoepassing om te controleren of deze waarde, anders een schadelijke service mogelijk een aanval **van bevoegdheden** veroorzaken <p><p> Voor het voorkomen van een aanval wordt u aangeraden om te bevestigen dat de `resource_id` overeenkomt met het grondtal voor het web API-URL die wordt geopend. Als https://service.contoso.com/data wordt geopend, bijvoorbeeld de `resource_id` htttps://service.contoso.com/ kan zijn. De clienttoepassing moet negeren een `resource_id` die begint niet met de basis-URL tenzij er is een betrouwbare alternatieve manier om te controleren of de-id. |

#### <a name="bearer-scheme-error-codes"></a>Vruchtdragende kleurenschema foutcodes

De specificatie RFC 6750 Hiermee definieert u de volgende fouten ziet voor resources waarmee via de WWW-verificatie kop- en vruchtdragende kleurenschema in de reactie.

| HTTP-statuscode | Foutcode | Beschrijving | Clientactie |
|------------------|------------|-------------|---------------|
| 400 | invalid_request | De aanvraag is niet juist opgemaakt. Dit is bijvoorbeeld een parameter ontbreekt of dezelfde parameter tweemaal gebruiken wordt. | De fout op te lossen en probeer opnieuw het verzoek. Dit soort fouten moet worden uitgevoerd alleen tijdens de ontwikkeling en in het eerste testen worden gedetecteerd. |
| 401 | invalid_token   | Het toegangstoken ontbreekt, ongeldige of is ingetrokken. De waarde van de parameter error_description biedt aanvullende informatie. |  Een nieuwe token aanvraagt bij de server autorisatie. Als het nieuwe token mislukt, is een onverwachte fout opgetreden. Een foutbericht wordt weergegeven voor de gebruiker en probeer het opnieuw verzenden na willekeurig vertragingen. |
| 403 | insufficient_scope | Het toegangstoken bevat geen de imitatie machtigingen vereist voor toegang tot de resource. | Een nieuwe autorisatie vergaderverzoek verzenden naar het eindpunt autorisatie. Als het antwoord de bereikparameter bevat, gebruikt u de bereikwaarde in de aanvraag voor de resource. |
| 403 | insufficient_access | Het onderwerp van het token beschikt niet over de machtigingen die nodig zijn om toegang tot de bron. | De gebruiker met een ander account of machtigingen voor de opgegeven bron aanvragen gevraagd. |

## <a name="refreshing-the-access-tokens"></a>De access-tokens vernieuwen

Access Tokens zijn tijdelijk en moeten worden vernieuwd nadat ze verlopen als u wilt doorgaan bronnen. U kunt vernieuwen de `access_token` door het indienen van een andere `POST` verzoek om deel te de `/token` eindpunt, maar deze keer leveren de `refresh_token` in plaats van de `code`.

Vernieuwen tokens ik heb geen opgegeven levensduur. De levensduur van vernieuwen tokens zijn meestal relatief lange. Echter in sommige gevallen vernieuwen tokens verloopt, worden ingetrokken of niet over voldoende bevoegdheden voor de gewenste actie. Uw toepassing moet verwachten en afhandelen van fouten die het resultaat van het eindpunt token uitgifte correct.

Wanneer u een antwoord met een fout vernieuwen ontvangt, de token van de huidige vernieuwen negeren en aanvragen van een nieuwe autorisatiecode of -toegangstoken. Met name wanneer een vernieuwing gebruiken token in de stroom autorisatie Code verlenen als u ontvangt een antwoord met het `interaction_required` of `invalid_grant` foutcodes, het vernieuwen token negeren en aanvragen van een nieuwe autorisatiecode.

Een steekproef aanvragen het eindpunt van de **tenant / regiospecifieke** (u kunt ook het **algemene** eindpunt) om een nieuwe access token met een token vernieuwen ziet er zo uit:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```
| Parameter | Beschrijving |
|-----------|-------------|
| access_token | Het nieuwe toegangstoken die is aangevraagd.|
| expires_in   | De resterende levensduur van het token in seconden. Een normale waarde is 3600 (één uur). |
| expires_on   | De datum en tijd waarop het token verloopt. De datum wordt weergegeven als het aantal seconden vanaf 1970-01-01T0:0:0Z UTC totdat de verlooptijd. |
| refresh_token | Een nieuwe OAuth 2.0-refresh_token die kunnen worden gebruikt om de nieuwe access tokens aanvragen wanneer dat in dit antwoord verloopt. |
| resource     | Geeft de beveiligde bron dat het toegangstoken kan worden gebruikt voor access. |
| bereik        | Imitatie machtigingen met de systeemeigen clienttoepassing. De standaardmachtiging is **user_impersonation**. De eigenaar van de doelbron kunt alternatieve waarden registreren in Azure AD. |
| token_type   | Het type token. De enige ondersteunde waarde is **vruchtdragende**. |

### <a name="successful-response"></a>Positief antwoord

Een positief token antwoord eruitziet:

```
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1460404526",
  "resource": "https://service.contoso.com/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "refresh_token": "AwABAAAAv YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl PM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfmVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA"
}
```

### <a name="error-response"></a>Foutmelding

Een antwoord van de fout steekproef kan er als volgt uit:

```
{
  "error": "invalid_resource",
  "error_description": "AADSTS50001: The application named https://foo.microsoft.com/mail.read was not found in the tenant named 295e01fc-0c56-4ac3-ac57-5d0ed568f872.  This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant.  You might have sent your authentication request to the wrong tenant.\r\nTrace ID: ef1f89f6-a14f-49de-9868-61bd4072f0a9\r\nCorrelation ID: b6908274-2c58-4e91-aea9-1f6b9c99347c\r\nTimestamp: 2016-04-11 18:59:01Z",
  "error_codes": [
    50001
  ],
  "timestamp": "2016-04-11 18:59:01Z",
  "trace_id": "ef1f89f6-a14f-49de-9868-61bd4072f0a9",
  "correlation_id": "b6908274-2c58-4e91-aea9-1f6b9c99347c"
}
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt om de soorten fouten die optreden classificeren en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar kunt identificeren de oorzaak van een verificatiefout.  |
| error_codes | Een lijst met STS specifieke foutcodes die in diagnostische hulpprogramma's kunnen helpen. |
| tijdstempel | De tijd waarop de fout is opgetreden. |
| trace_id | Een unieke id voor de aanvraag die u kunt bij het oplossen van diagnostische gegevens.  |
| correlation_id | Een unieke id voor de aanvraag die u kunt bij het oplossen van diagnostische gegevens over de onderdelen.|

Voor een beschrijving van de foutcodes en de actie aanbevolen client, raadpleegt u [foutcodes voor token-eindpunt fouten](#error-codes-for-token-endpoint-errors).
