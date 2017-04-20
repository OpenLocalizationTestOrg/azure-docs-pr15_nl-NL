<properties
    pageTitle="Aan de slag met Azure Active Directory identiteitsbeveiliging en Microsoft Graph | Microsoft Azure"
    description="Biedt een inleiding tot query Microsoft Graph voor een lijst met risico's en de bijbehorende gegevens van Azure Active Directory."
    services="active-directory"
    keywords="beveiliging van Azure active directory-identiteit, onvoorziene gebeurtenis, beveiligingsprobleem, beveiligingsbeleid, Microsoft Graph"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Aan de slag met Azure Active Directory identiteitsbeveiliging en Microsoft Graph

Microsoft Graph is Microsofts geïntegreerd API eindpunt en de start van de [Azure Active Directory identiteitsbeveiliging van](active-directory-identityprotection.md) API's. Onze eerste API, **identityRiskEvents**, kunt u query Microsoft Graph voor een lijst met [risico's](active-directory-identityprotection-risk-events-types.md) en de bijbehorende gegevens. In dit artikel krijgt u te helpen bij het controleren van deze API. Voor een in diepte inleiding, volledige documentatie en toegang tot de Explorer Graph, raadpleegt u de [Microsoft Graph-site](https://graph.microsoft.io/).

Er zijn drie stappen identiteitsbeveiliging gegevenstoegang via Microsoft Graph:

1. Een toepassing met een client geheim toevoegen. 

2. Gebruik deze geheim en enkele andere soorten informatie om te verifiëren naar Microsoft Graph, waar u een verificatietoken ontvangt. 

3. Gebruik deze token aanvragen aanbrengen in het API-eindpunt en identiteitsbeveiliging gegevens terug.

Voordat u begint, moet u op:

- Beheerdersbevoegdheden maken van de toepassing in Azure AD
- De naam van uw tenant domein (bijvoorbeeld contoso.onmicrosoft.com)



## <a name="add-an-application-with-a-client-secret"></a>Een toepassing met een client geheim toevoegen


1. [Meld u aan](https://manage.windowsazure.com) bij uw Azure klassieke-portal als beheerder. 

1. Klik op in het linkernavigatiedeelvenster op **Active Directory**. 

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. In het menu aan de bovenkant, klikt u op **toepassingen**.

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing het ontwikkelen van mijn organisatie toevoegen**.

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)


5. Klik in het dialoogvenster **Vertel ons over het gebruik van de toepassing** kunt u de volgende stappen uitvoeren:

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)

    een. Typ in het tekstvak **naam** een naam voor uw toepassing (bijvoorbeeld: AADIP risico gebeurtenis API van toepassing).

    b. Als het **Type**, selecteert u de **Web-toepassing en/of Web API**.

    c. Klik op **volgende**.


5. Klik in het dialoogvenster **Eigenschappen van de App** kunt u de volgende stappen uitvoeren:

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)

    een. Typ in het tekstvak **URL-aanmelding** `http://localhost`.

    b. Typ in het tekstvak **App-ID-URI** `http://localhost`.

    c. Klik op **Voltooien**.


U kunt nu uw toepassing configureren.

![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)



## <a name="grant-your-application-permission-to-use-the-api"></a>Uw toepassing toestemming verlenen om te gebruiken van de API


1. Klik op de pagina van de toepassing in het menu aan de bovenkant, klikt u op **configureren**. 

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)

2. Klik op **toevoegen toepassing**in de sectie **machtigingen voor andere toepassingen** .

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)


2. Klik in het dialoogvenster **machtigingen voor andere toepassingen** , moet u de volgende stappen uitvoeren:

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)

    een. Selecteer **Microsoft Graph**.

    b. Klik op **Voltooien**.


1. Klik op **machtigingen van toepassing: 0**, en selecteert u **alle gegevens van het risico gebeurtenis identiteit lezen**.

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)

1. Klik op **Opslaan** onder aan de pagina.

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)


## <a name="get-an-access-key"></a>Een access-sleutel ophalen

1. Selecteer op de pagina van de toepassing, klik in de sectie **toetsen** 1 jaar als duur.

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)

1. Klik op **Opslaan** onder aan de pagina.

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

1. Klik in de sectie toetsen de waarde van de zojuist gemaakte sleutel Kopieer en plak deze in een veilige locatie.

    ![Een toepassing maken](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)

    > [AZURE.NOTE] Als u deze toets kwijtraakt, wordt u terug naar deze sectie en maak een nieuwe sleutel moet. Houd deze toets een geheim: iedereen met deze toegang heeft tot uw gegevens.


1. In de sectie **Eigenschappen** kopieert u de **Client-ID**en plak deze in een veilige locatie. 


## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Verifiëren naar Microsoft Graph en de identiteit risico gebeurtenissen API query

Nu nodig u hebt:

- De client-ID die u hierboven hebt gekopieerd

- De sleutel die u hierboven hebt gekopieerd

- De naam van uw tenant domein


Als u wilt verifiëren, verzenden naar een post-aanvraag `https://login.microsoft.com` met de volgende parameters in de hoofdtekst:

- grant_type: "**client_credentials**"

- resource: "**https://graph.microsoft.com**"

- client_id:<your client ID>

- client_secret:<your key>


> [AZURE.NOTE] U moet opgeven van waarden voor de **client_id** en de parameter **client_secret** .

Als resultaat oplevert, dit een verificatietoken.  
Als u wilt bellen de API, maakt u een koptekst met de volgende parameter:

    `Authorization`=”<token_type> <access_token>"


Wanneer er wordt geverifieerd, kunt u het type token en toegangstoken kunt vinden in de resulterende token.

Deze koptekst als een nieuw vergaderverzoek verzenden naar de volgende API-URL:`https://graph.microsoft.com/beta/identityRiskEvents`

Het antwoord, is als dit lukt, een verzameling identiteit risico's en de bijbehorende gegevens in de OData-JSON-indeling die kan worden geparseerd en verwerkt als naar eigen inzicht.

Hier ziet u voorbeelden van code voor de verificatie en bellen van de API via Powershell.  
Uw klant-ID, toe te voegen belangrijke en domein tenant.

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 


## <a name="next-steps"></a>Volgende stappen

Gefeliciteerd, u alleen de eerste aanroep van Microsoft Graph hebt aangebracht.  
U kunt nu query identiteit risico's en gebruikt u de gegevens maar naar eigen inzicht.

Meer informatie over Microsoft Graph en hoe u met de API Graph toepassingen bouwen, Controleer de [documentatie](https://graph.microsoft.io/docs) en nog veel meer op de [Microsoft Graph-site](https://graph.microsoft.io/). Zorg er ook naar de pagina [Azure AD identiteit beveiliging API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) waarin alleoffice van de identiteit beveiliging-API's beschikbaar in de grafiek met een bladwijzer markeren. Terwijl we nieuwe manieren om te werken met identiteitsbeveiliging via API hebt toegevoegd, ziet u ze op die pagina.


## <a name="additional-resources"></a>Aanvullende informatie

- [Beveiliging van Azure Active Directory-identiteit](active-directory-identityprotection.md)

- [Typen risico gebeurtenissen gedetecteerd door Azure Active Directory identiteitsbeveiliging](active-directory-identityprotection-risk-events-types.md)

- [Microsoft Graph](https://graph.microsoft.io/)

- [Overzicht van Microsoft Graph](https://graph.microsoft.io/docs)

- [Azure AD-identiteit beveiliging Service hoofdsite](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)
