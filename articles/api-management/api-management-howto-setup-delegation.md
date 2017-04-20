<properties 
    pageTitle="Hoe u een gemachtigde gebruiker registratie en product-abonnement" 
    description="Leer hoe u een gemachtigde gebruiker registratie en product-abonnement aan een derde partij in Azure API Management." 
    services="api-management" 
    documentationCenter="" 
    authors="antonba" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="antonba"/>

# <a name="how-to-delegate-user-registration-and-product-subscription"></a>Hoe u een gemachtigde gebruiker registratie en product-abonnement

Delegeren kunt u uw bestaande website gebruiken voor het afhandelen van ontwikkelaars Aanmeldingsadres-in/sign-up-to-date en een abonnement op producten in plaats van de ingebouwde functie in de portal voor ontwikkelaars. Hiermee worden uw website naar de eigenaar bent van de gebruikersgegevens en de validatie van de volgende stappen uit in een aangepaste manier uitvoeren.

## <a name="delegate-signin-up"> </a>Overdragen ontwikkelaars aanmelden en aanmelding

Als u wilt delegeren ontwikkelaars aanmelden en aanmelden bij uw bestaande website moet u een speciaal delegeren om eindpunt te maken van uw site die als het ingangspunt voor het verzoek gestart vanaf de API Management developer portal fungeert.

De uiteindelijke werkstroom worden als volgt:

1. Ontwikkelaars klikken op de koppeling aanmelden of registreren de API Management developer portal
2. Browser wordt omgeleid naar het eindpunt delegeren
3. Delegeren eindpunt in wordt omgeleid naar of presenteert UI-gebruiker aanmelden of registreren vragen
4. Bij voltooiing, wordt de gebruiker omgeleid terug naar de API Management ontwikkelaars portalpagina die ze uit de slag


Als u wilt beginnen, aanvraagt laten we eerste ingesteld API Management om te leiden via uw eindpunt delegeren. Klik op **beveiliging** en klik vervolgens op het tabblad **overdracht** in de API Management publisher-portal. Klik op het selectievakje om in te schakelen van de gemachtigde aanmelden en aanmelding.

![Pagina Delegation for domein][api-management-delegation-signin-up]

* Bepalen wat de URL van uw eindpunt speciale delegeren worden en voert u het in het veld **delegeren eindpunt URL** . 

* Voer in het veld **delegeren verificatiesleutel** een geheim dat wordt gebruikt voor het berekenen van een handtekening die u hebt voor de verificatie om ervoor te zorgen dat de aanvraag is daadwerkelijk die afkomstig zijn uit Azure API Management. U kunt Klik op de knop **genereren** als u wilt dat de API Managemnet willekeurig sleutel genereren voor u.

Nu u nodig hebt om de **overdracht eindpunt**te maken. Als deze is een aantal acties uitvoeren:

1. Een aanvraag ontvangt in de volgende vorm:

    > *http://www.yourwebsite.com/apimdelegation?operation=Signin&returnUrl= {URL van bronpagina} & zout = {tekenreeks} & sig = {tekenreeks}*

    Queryparameters voor de zaak aanmelden / registreren:
    - **bewerking**: aangeeft welk type taakoverdracht aan dit aanvraagt is: het kan **aanmelding bij** alleen in dit geval zijn
    - **returnUrl**: de URL van de pagina waar de gebruiker hebt geklikt op een koppeling aanmelden of registreren
    - **zout**: een speciale tekenreeks van het gebruikt voor een waardepapier hash computing
    - **SIG**: een hash berekende beveiliging moet worden gebruikt voor vergelijking met uw eigen berekende hash

2. Controleer of dat de aanvraag is afkomstig van Azure API Management (optioneel, maar sterk aanbevolen voor beveiliging)

    * Een HMAC-SHA512-hash van een tekenreeks op basis van de queryparameters **returnUrl** en **zout** ([voorbeeldcode hieronder]) berekenen:
        > HMAC (**zout** + '\n' + **returnUrl**)
         
    * Vergelijk de boven-berekende hash aan de waarde van de queryparameter **sig** . Als de twee hashes overeenkomen, gaat u naar de volgende stap, anders kan de aanvraag weigeren.

2. Verifiëren dat u een verzoek voor aanmelding-in/Aanmeldingsadres-up ontvangt: de **bewerking** queryparameter wordt ingesteld op ' bij**aanmelding bij**'.

3. De gebruiker presenteren met UI aanmelden of registreren

4. Als de gebruiker zich aanmeldt-up die u moet een bijbehorende account voor hen in API Management maken. [Maak een gebruiker] met de API Management REST API. Zorg ervoor dat u de gebruikers-ID instellen naar hetzelfde is in uw archief van de gebruiker of een id die u kunt bijhouden waar als als u dit doet.

5. Wanneer de gebruiker is geverifieerd:

    * [een eenmalige aanmelding (SSO) token aanvragen] via de API Management REST API

    * een queryparameter returnUrl toevoegen aan de SSO-URL die u hebt ontvangen van de API-aanroep hierboven:
        > bijvoorbeeld https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url 

    * de gebruiker omleiden naar de bovenstaande geproduceerd URL

Naast de **aanmelding bij** betrekking heeft, kunt u ook accountbeheer door de vorige stappen en het gebruik van een van de volgende bewerkingen uitvoeren.

-   **ChangePassword**
-   **ChangeProfile**
-   **CloseAccount**

U moet de volgende parameters query van het account beheer.

-   **bewerking**: aangeeft welk type verzoek om taakoverdracht is (ChangePassword, ChangeProfile of CloseAccount)
-   **gebruikers-id**: de gebruikers-id van het account beheren
-   **zout**: een speciale tekenreeks van het gebruikt voor een waardepapier hash computing
-   **SIG**: een hash berekende beveiliging moet worden gebruikt voor vergelijking met uw eigen berekende hash

## <a name="delegate-product-subscription"> </a>Delegeren van product-abonnement

Delegeren van product-abonnement werkt op dezelfde manier naar het delegeren van gebruiker aanmelden /-up. De uiteindelijke werkstroom zou als volgt:

1. Ontwikkelaars Hiermee selecteert u een product in de API Management developer portal en klikt op de knop abonneren
2. Browser wordt omgeleid naar het eindpunt delegeren
3. Delegeren eindpunt vereiste product abonnement stappen uitvoert: dit is aan u en kan leiden tot omleiden naar een andere pagina factureringsgegevens extra vragen, of gewoon de informatie op te slaan en geen aanmelding vereist een gebruikersactie aanvragen


Als u wilt de functionaliteit inschakelen, klikt u op **gemachtigde product abonnement**op de pagina **Delegation** .

Controleer dat het eindpunt delegeren kunt u de volgende acties uitvoeren:


1. Een aanvraag ontvangt in de volgende vorm:

    > *http://www.yourwebsite.com/apimdelegation?operation= {bewerking} & product-id = {product zich kunnen abonneren op} & ID = {gebruiker aanvraag} & zout = {tekenreeks} & sig = {tekenreeks}*

    Queryparameters voor het geval van product-abonnement:
    - **bewerking**: wordt aangegeven welk type verzoek om taakoverdracht deze. Voor product abonnement zijn aanvragen de geldige opties:
        - "Abonneren": een verzoek om de gebruiker naar een bepaald product met een abonnement nemen opgegeven ID (Zie hieronder)
        - "Afmelden": een verzoek om een gebruiker van een product afmelden
        - "Verlengen": een verzoek om een abonnement (bijvoorbeeld die mogelijk worden verlopend) te verlengen
    - **product-id**: de ID van het product dat de gebruiker gevraagd om u te abonneren op
    - **gebruikers-id**: de ID van de gebruiker voor wie het verzoek is gedaan
    - **zout**: een speciale tekenreeks van het gebruikt voor een waardepapier hash computing
    - **SIG**: een hash berekende beveiliging moet worden gebruikt voor vergelijking met uw eigen berekende hash


2. Controleer of dat de aanvraag is afkomstig van Azure API Management (optioneel, maar sterk aanbevolen voor beveiliging)

    * Een HMAC-SHA512 van een tekenreeks op basis van de **product-id**, **gebruikers-id** en **zout** queryparameters berekenen:
        > HMAC (**zout** '\n' + **product-id** + '\n' + **gebruikers-id**)
         
    * Vergelijk de boven-berekende hash aan de waarde van de queryparameter **sig** . Als de twee hashes overeenkomen, gaat u naar de volgende stap, anders kan de aanvraag weigeren.
    
3. Elk product abonnement bewerkingen op basis van het type gevraagde in **bewerking** - bijvoorbeeld facturering verder vragen, enzovoort bewerking uitvoeren.

4. Klik op abonneren is de gebruiker het product aan uw kant, Abonneer u de gebruiker het product API Management door de [ondersteuning voor de REST API voor product-abonnement].

## <a name="delegate-example-code"></a> Voorbeeldcode ##

Deze codevoorbeelden wordt het maken van de *overdracht validatiesleutel*is ingesteld in het scherm doorschakelen naar gemachtigden van de publisher-portal op een HMAC die vervolgens wordt gebruikt voor het valideren van de handtekening, aan te tonen dat de geldigheid van de doorgegeven returnUrl maken. Dezelfde code werkt voor de product-id en gebruikers-id met kleine wijziging.

**C#-code genereren hash van returnUrl**

    using System.Security.Cryptography;

    string key = "delegation validation key";
    string returnUrl = "returnUrl query parameter";
    string salt = "salt query parameter";
    string signature;
    using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
    {
        signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
        // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
        // compare signature to sig query parameter
    }


**NodeJS-code genereren hash van returnUrl**

    var crypto = require('crypto');
    
    var key = 'delegation validation key'; 
    var returnUrl = 'returnUrl query parameter';
    var salt = 'salt query parameter';
    
    var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
    var digest = hmac.update(salt + '\n' + returnUrl).digest();
    // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature to sig query parameter
    
    var signature = digest.toString('base64');

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over delegeren, de volgende video.

> [AZURE.VIDEO delegating-user-authentication-and-product-subscription-to-a-3rd-party-site]

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[verzoek om een token eenmalige aanmelding (SSO)]: http://go.microsoft.com/fwlink/?LinkId=507409
[een gebruiker maken]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser
[bellen van de REST API voor product-abonnement]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO
[Next steps]: #next-steps
[Hieronder vindt u voorbeeldcode]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 