###<a name="server-auth"></a>Hoe u: verificatie met een Provider (Server stroom)

Als u wilt beheren van het verificatieproces in uw app Mobile-Apps hebt, moet u uw app met uw identiteitsprovider registreren. Klik in uw App Azure-Service moet u de toepassings-ID en geheim verstrekt door uw provider configureren.
Voor meer informatie raadpleegt u de zelfstudie [verificatie toevoegen aan uw app].

Nadat u uw identiteitsprovider hebt geregistreerd, belt u gewoon de methode .login() met de naam van uw provider. Als u bijvoorbeeld aanmelden met Facebook-gebruik van de volgende code.

```
client.login("facebook").done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

Als u een identiteitsprovider dan Facebook gebruikt, wijzigt u de waarde doorgegeven aan de bovenstaande methode login naar een van de volgende handelingen uit: `microsoftaccount`, `facebook`, `twitter`, `google`, of `aad`.

Azure-App-Service beheert in dit geval de stroom van de verificatie OAuth 2.0 door het weergeven van de aanmeldingspagina van de geselecteerde provider en een App Service verificatietoken genereren na succesvolle Meld u aan met de identiteitsprovider. De functie login als alles compleet is, geeft als resultaat een JSON-object (gebruiker) die de gebruikers-ID en de App Service verificatietoken in de velden gebruikersnaam en authenticationToken respectievelijk. Deze token en kan worden opgeslagen in de cache opnieuw gebruikt totdat het verloopt.

###<a name="client-auth"></a>Hoe u: verificatie met een Provider (Client-stroom)

Uw app kan ook onafhankelijk contact opnemen met de identiteitsprovider en geef de resulterende token uw App-service voor verificatie. Deze stroom client kunt u een functionaliteit voor eenmalige aanmelding om gebruikers te bieden of extra gebruikersgegevens ophalen uit de identiteitsprovider.

#### <a name="social-authentication-basic-example"></a>Voorbeeld van sociale verificatie de eenvoudige

In dit voorbeeld wordt de Facebook-client SDK gebruikt voor verificatie:

```
client.login(
     "facebook",
     {"access_token": token})
.done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```
In dit voorbeeld wordt ervan uitgegaan dat de token dat is opgegeven door de desbetreffende provider SDK is opgeslagen in de token variabele.

#### <a name="microsoft-account-example"></a>Voorbeeld van de Microsoft-Account

Het volgende voorbeeld wordt de SDK Live, die ondersteuning biedt voor één-aanmelding voor Windows Store-apps met behulp van Microsoft-Account:

```
WL.login({ scope: "wl.basic"}).then(function (result) {
      client.login(
            "microsoftaccount",
            {"authenticationToken": result.session.authentication_token})
      .done(function(results){
            alert("You are now logged in as: " + results.userId);
      },
      function(error){
            alert("Error: " + err);
      });
});
```

In dit voorbeeld wordt een token van Live verbinding kunt maken, die door het aanroepen van de functie login uw App-service is geleverd.

###<a name="auth-getinfo"></a>Hoe u: informatie over de geverifieerde gebruiker

De verificatiegegevens voor de huidige gebruiker kan worden opgehaald uit de `/.auth/me` eindpunt met behulp van een AJAX-methode.  Controleer of u instellen dat de `X-ZUMO-AUTH` waarop u wilt uw authenticatie token.  Het verificatietoken is opgeslagen in `client.currentUser.mobileServiceAuthenticationToken`.  Als u bijvoorbeeld de ophalen API gebruiken:

```
var url = client.applicationUrl + '/.auth/me';
var headers = new Headers();
headers.append('X-ZUMO-AUTH', client.currentUser.mobileServiceAuthenticationToken);
fetch(url, { headers: headers })
    .then(function (data) {
        return data.json()
    }).then(function (user) {
        // The user object contains the claims for the authenticated user
    });
```

Ophalen is beschikbaar als een pakket met npm of browser downloaden van CDNJS. U kunt ook jQuery of een ander AJAX-API kunt u de gegevens ophalen.  Gegevens worden ontvangen als een object JSON.
