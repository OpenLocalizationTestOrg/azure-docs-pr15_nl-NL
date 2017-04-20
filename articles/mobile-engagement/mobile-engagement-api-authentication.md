<properties 
    pageTitle="Verificatie met mobiele betrokkenheid REST API 's"
    description="Wordt uitgelegd hoe u om te verifiëren met Azure Mobile betrokkenheid REST API 's" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="10/05/2016"
    ms.author="wesmc;ricksal"/>

# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Verificatie met mobiele betrokkenheid REST API 's

## <a name="overview"></a>Overzicht

In dit document wordt beschreven hoe u een geldige AAD Oauth token om te verifiëren met de mobiele betrokkenheid REST API's. 

Er wordt ervan uitgegaan dat u een geldig abonnement op Azure hebt en u een betrokkenheid van de Mobile-app met een van onze [Ontwikkelaars zelfstudies](mobile-engagement-windows-store-dotnet-get-started.md)hebt gemaakt.

## <a name="authentication"></a>Verificatie

Een Microsoft Azure Active Directory gebaseerd OAuth token wordt gebruikt voor verificatie. 

In de volgorde verificatie een API-aanvraag, moet een koptekst autorisatie worden toegevoegd aan elke aanvraag dat wil de volgende notatie zeggen:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

>[AZURE.NOTE] Azure Active Directory-tokens verlopen in 1 uur staan.

Er zijn verschillende manieren om een token. Aangezien de API's gewoonlijk vanuit een cloudservice aangeroepen worden, die u wilt een API-sleutel gebruiken. Een API-sleutel in de Azure terminologie is een Service principal wachtwoord genoemd. De volgende procedure beschreven één manier voor het handmatig instellen.

### <a name="one-time-setup-using-script"></a>Eenmalige instelling (met behulp van script)

Volg de onderstaande instructies voor het uitvoeren van de configuratie met een PowerShell-script wat de minimale duurt voor installatie, maar gebruikt de meest toegestane standaardinstellingen set. U kunt desgewenst ook Volg de instructies in de [handmatige instelling](mobile-engagement-api-authentication-manual.md) voor dit rechtstreeks vanuit de Azure-portal doen en betere configuratie doen. 

1. De nieuwste versie van Azure PowerShell krijgen van [hier](http://aka.ms/webpi-azps). Voor meer informatie over de instructies voor het downloaden, kunt u deze [koppeling](../powershell-install-configure.md)zien.  

2. Wanneer Azure PowerShell is geïnstalleerd, kunt u de volgende opdrachten gebruiken om ervoor te zorgen dat u de **Azure module** hebt geïnstalleerd hebt:

    een. Controleer of dat de Azure PowerShell-module is beschikbaar in de lijst met beschikbare modules. 
    
        Get-Module –ListAvailable 

    ![Beschikbare Azure Modules][1]
        
    b. Als u de Azure PowerShell-module niet in de bovenstaande lijst vinden moet u het volgende:
        
        Import-Module Azure 
        
3. Aanmelden bij de Azure Resource Manager vanaf PowerShell door de volgende opdracht en waarin u uw gebruikersnaam en wachtwoord voor uw Azure-account: 
        
        Login-AzureRmAccount

4. Als u meerdere abonnementen hebt en vervolgens moet u de volgende handelingen uit:

    een. Een lijst met alle abonnementen en kopieer de SubscriptionId van het abonnement dat u wilt gebruiken. Controleer of dat dit abonnement is dezelfde een waarvoor de Mobile-App voor betrokkenheid die u werken wilt met het gebruik van de API's. 

        Get-AzureRmSubscription

    b. Voer de volgende opdracht leveren van de SubscriptionId voor het configureren van het abonnement dat moet worden gebruikt.

        Select-AzureRmSubscription –SubscriptionId <subscriptionId>

5. De tekst voor het script [Nieuw AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) naar uw lokale computer kopiëren en opslaan als een PowerShell-cmdlet (bijvoorbeeld `APIAuth.ps1`) en uitvoeren van deze `.\APIAuth.ps1`. 
    
6. Het script vraagt u een invoertaal voor **principalName**opgeeft. Geef een passende naam hier dat u wilt gebruiken om te maken van uw Active Directory-toepassing (bijvoorbeeld APIAuth). 

7. Nadat het script is voltooid, verschijnt het volgende vier waarden die u nodig hebt om te verifiëren via programmacode met AD Controleer dus of om deze te kopiëren. 
        
    **TenantId**, **SubscriptionId**, **ApplicationId**en **geheim**.

    Gewenste TenantId als `{TENANT_ID}`, ApplicationId als `{CLIENT_ID}` en geheime als `{CLIENT_SECRET}`.

    > [AZURE.NOTE] Uw standaardbeveiligingsbeleid die mogelijk voorkomen dat u uitvoeren van een PowerShell-scripts. Zo ja, configureren u tijdelijk uw beleid script worden uitgevoerd met de volgende opdracht:

        > Set-ExecutionPolicy RemoteSigned

8. Hier ziet u hoe de set PS cmdlets eruitziet. 

    ![][3]

9. Schakel in de beheerportal van Azure dat een nieuwe AD-toepassing is gemaakt met de naam die u aan het script **principalName** onder **dat weergeven toepassingen mijn bedrijf eigenaar is**genoemd.

    ![][4]

#### <a name="steps-to-get-a-valid-token"></a>Stappen voor het ophalen van een geldige token

1. De API met de volgende parameters bellen en zorg ervoor dat voor het vervangen van de TENANT\_-ID, CLIENT\_-ID en -CLIENT\_GEHEIM:

    - **URL aanvragen** als *https://login.microsoftonline.com/ {TENANT\_ID} / oauth2/token*
    - **HTTP-inhoudstype koptekst** als *toepassing/x-1-800-www-Dell / form-urlencoded*
    - **HTTP-aanvragen hoofdtekst** zo *verlenen\_type = client\_referenties & client_id = {CLIENT\_ID} & client_secret = {CLIENT\_GEHEIM} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*

    Hieronder ziet u een voorbeeld-verzoek.

        POST /{TENANT_ID}/oauth2/token HTTP/1.1
        Host: login.microsoftonline.com
        Content-Type: application/x-www-form-urlencoded
        grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&reso
        urce=https%3A%2F%2Fmanagement.core.windows.net%2F

    Hier volgt een voorbeeld antwoord wordt verzonden:

        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Content-Length: 1234
    
        {"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144
        5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}

    In dit voorbeeld opgenomen URL-codering van de parameters van het bericht, `resource` waarde is werkelijk `https://management.core.windows.net/`. Pas op dat ook URL coderen `{CLIENT_SECRET}` mogelijk deze speciale tekens bevatten.

    > [AZURE.NOTE] Voor het testen, kunt u een hulpprogramma voor HTTP-client zoals [Fiddler](http://www.telerik.com/fiddler) of [Chrome Postman-extensie](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) 

2. Neem nu de koptekst van de aanvraag autorisatie in elke API-oproep:

        Authorization: Bearer {ACCESS_TOKEN}

    Als u een geretourneerde 401 statuscode ontvangen, controleert u de tekst voor het antwoord deze kan u vertellen het token is verlopen. In dat geval geen nieuwe token ophalen.

##<a name="using-the-apis"></a>Met de API 's

Nu dat u een geldig token hebt, bent u gereed om de oproepen API maken.

1. In elk verzoek om een API moet u een geldige, niet verlopen token waarmee u verkregen doorgeven in de vorige sectie.

2. U moet enkele parameters in het verzoek URI waarmee uw toepassing invoegtoepassing. Het verzoek URI ziet er als volgt

        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/

    Parameters ophalen, klikt u op de naam van uw toepassing en klik op Dashboard en u ziet een pagina als volgt te werk met de 3 parameters.

    - **1**`{subscription-id}`
    - **2**`{app-collection}`
    - **3**`{app-resource-name}`
    - **4** de naam van uw resourcegroep is het verstandig om **MobileEngagement** tenzij u een nieuw gemaakt. 

    ![Mobile betrokkenheid API URI parameters][2]

>[AZURE.NOTE] <br/>
>1. Negeer het adres van de hoofdsite API zoals deze voor de vorige API's was.<br/>
>2. Als u de app met behulp van Azure klassieke portal gemaakt moet u de naam van de toepassing Resource dat anders dan de naam van de toepassing is te gebruiken. Als u de app hebt gemaakt in de Portal Azure kunt u de naam van de App zelf (er wordt geen onderscheid te maken tussen de naam van de Resource-toepassing en de naam van de App voor apps die zijn gemaakt in de nieuwe portal) moet gebruiken.  

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png



