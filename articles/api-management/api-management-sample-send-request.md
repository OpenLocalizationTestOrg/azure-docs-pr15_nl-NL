<properties
    pageTitle="API-beheerservice gebruiken om u te genereren HTTP-aanvragen"
    description="Leer hoe u beleidsregels voor vergaderverzoeken en antwoorden in API Management met externe services bellen vanuit uw API"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>


# <a name="using-external-services-from-the-azure-api-management-service"></a>Met behulp van externe services van de Azure API Management-service

Het beleid van Azure API Management-service kan een groot aantal handige werk op basis zuiver van de binnenkomende aanvraag, de uitgaande antwoord en eenvoudige configuratiegegevens. Echter wordt kunnen werken met externe services uit het beheer van de API beleidsregels wordt geopend veel meer mogelijkheden.

We hebben eerder gezien hoe we kunnen werken met de [service Azure gebeurtenis Hub voor logboekregistratie, bewaken en analyses](api-management-log-to-eventhub-sample.md). In dit artikel wordt gedemonstreerd beleidsregels waarmee u kunt werken met een externe HTTP op basis van service. Dit beleid kunnen worden gebruikt voor externe gebeurtenissen activeert of voor het ophalen van de informatie die wordt gebruikt voor het bewerken van de oorspronkelijke vergaderverzoeken en antwoorden op enkele manier.

## <a name="send-one-way-request"></a>Een-manier-verzoek om te verzenden
Mogelijk de eenvoudigste externe interactie is de stijl fire en vergeet van aanvraag kunt invullen waarmee een externe service moeten meldingen ontvangen van een soort belangrijke gebeurtenis. We de beschikking over het beleid van de stroom besturingselement `choose` om te bepalen van elk type voorwaarde dat we geïnteresseerd bent in en klik vervolgens, als de voorwaarde is voldaan, kunnen we een externe HTTP-aanvraag met het beleid [een-manier-verzoek om te verzenden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) . Dit kan een aanvraag om een berichtensysteem zoals Hipchat of marge of een mail API zoals SendGrid of MailChimp, of voor kritieke ondersteuningsaanvragen bijvoorbeeld PagerDuty. Al deze SMS systemen hebben eenvoudige HTTP-APIs die we eenvoudig kunt aanroepen.

### <a name="alerting-with-slack"></a>Waarschuwingen met toegestane vertraging
Het volgende voorbeeld wordt gedemonstreerd hoe u een bericht verzenden naar een chatruimte tekort als de HTTP-antwoord statuscode groter dan of gelijk is aan 500 is. Een bereikfout 500 geeft een probleem met onze backend API die de client van onze API zelf niet kunt oplossen. Dit is een soort tussenkomst onze gedeelte meestal vereist.  

    <choose>
        <when condition="@(context.Response.StatusCode >= 500)">
          <send-one-way-request mode="new">
            <set-url>https://hooks.slack.com/services/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</set-url>
            <set-method>POST</set-method>
            <set-body>@{
                    return new JObject(
                            new JProperty("username","APIM Alert"),
                            new JProperty("icon_emoji", ":ghost:"),
                            new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",
                                                    context.Request.Method,
                                                    context.Request.Url.Path + context.Request.Url.QueryString,
                                                    context.Request.Url.Host,
                                                    context.Response.StatusCode,
                                                    context.Response.StatusReason,
                                                    context.User.Email
                                                    ))
                            ).ToString();
                }</set-body>
          </send-one-way-request>
        </when>
    </choose>

Toegestane vertraging heeft het begrip van de binnenkomende web haken. Tijdens het configureren van een haakje inkomende web, genereert toegestane vertraging van de URL van een speciaal waarmee u een eenvoudige POST-aanvraag en doorgeven van een bericht in het tekort kanaal. De JSON-instantie die we maken is gebaseerd op een indeling die is gedefinieerd door de marge.

![Toegestane vertraging Web haakje](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>Fire is en vergeet goed?
Er zijn bepaalde compromissen nodig zijn bij het gebruik van een stijl fire en vergeet van aanvraag kunt invullen. Het verzoek mislukt als om een bepaalde reden en de fout niet worden gerapporteerd. In dit geval, is de complexiteit van met een secundaire fout rapportage-systeem en de prestaties van extra kosten van het wachten op het antwoord niet nodig. Voor scenario's waarop de invoegtoepassing is essentiële om te controleren van het antwoord, is het beleid [verzoek om te verzenden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) klikt u vervolgens een betere optie.

## <a name="send-request"></a>Verzoek om verzenden
De `send-request` beleid schakelt u het gebruik van een externe service voor complexe verwerkingsfuncties uitvoeren en retourneren gegevens met de API management service-die kunnen worden gebruikt voor verdere beleidsverwerking.

### <a name="authorizing-reference-tokens"></a>Verwijzingstokens machtigen
Een primaire functie API beheer van beveiligt backend resources. Als autorisatie de server die wordt gebruikt door uw API [JWT tokens](http://jwt.io/) als onderdeel van de stroom is OAuth2, maakt evenals [Azure Active Directory](../active-directory/active-directory-aadconnect.md) , en vervolgens kunt u de `validate-jwt` beleid om te controleren of de geldigheid van het token. Sommige autorisatie-servers maken echter zogenaamde [verwijzingstokens](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) die kan niet worden gecontroleerd zonder dat een oproep terug naar de server autorisatie.

### <a name="standardized-introspection"></a>Gestandaardiseerde introspection
In het verleden is er is geen gestandaardiseerde manier van een sleutel verwijzing met een autorisatie-server is geverifieerd. Een recent voorgestelde standaard [RFC 7662](https://tools.ietf.org/html/rfc7662) is echter gepubliceerd door de IETF waarmee wordt gedefinieerd hoe een resource-server kunt controleren of de geldigheid van een token.

### <a name="extracting-the-token"></a>Voor het extraheren van de token
De eerste stap is het token extraheren uit de koptekst autorisatie. Waarde van de header moet worden opgemaakt met de `Bearer` autorisatie kleurenschema, een spatie en klikt u vervolgens het Autorisatietoken aan de hand van [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1). Er zijn helaas gevallen waarin het autorisatieschema wordt weggelaten. Om rekening te dit bij het parseren, we de waarde van de header op een spatie splitsen en selecteer de laatste tekenreeks in de resulterende matrix van reeksen. Hierdoor wordt een tijdelijke oplossing voor onjuist opgemaakte autorisatie kopteksten.

    <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

### <a name="making-the-validation-request"></a>De aanvraag gegevensvalidatie
Als we de Autorisatietoken hebben, kunnen we de aanvraag voor het valideren van het token. RFC 7662 dit proces introspection belt en vereist dat u `POST` een HTML-formulier aan iedere resource introspection. Het HTML-formulier moet ten minste twee sleutelwaarde/bevatten met de toets `token`. Deze aanvraag op de server autorisatie moet ook worden geverifieerd om ervoor te zorgen dat kwaadwillende clients niet kunnen voor geldige tokens sleepnetten gaan.

    <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
      <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
      <set-method>POST</set-method>
      <set-header name="Authorization" exists-action="override">
        <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
      </set-header>
      <set-header name="Content-Type" exists-action="override">
        <value>application/x-www-form-urlencoded</value>
      </set-header>
      <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
    </send-request>

### <a name="checking-the-response"></a>Het antwoord controleren
De `response-variable-name` kenmerk wordt gebruikt om toegang te geven het geretourneerde antwoord. De naam die is gedefinieerd in deze eigenschap kan worden gebruikt als een sleutel in de `context.Variables` woordenlijst voor toegang tot de `IResponse` object.

Van het antwoordobject we de hoofdtekst kunt ophalen en RFC 7622 leest ons dat het antwoord een JSON-object en moet ten minste een eigenschap met de naam bevatten `active` dat wil zeggen een Booleaanse waarde. Wanneer `active` waar is en vervolgens het token wordt beschouwd als geldig.

### <a name="reporting-failure"></a>Foutmelding
We gebruiken een `<choose>` beleid om te bepalen of het token ongeldig is en zo ja, een 401 antwoord retourneren.

    <choose>
      <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
        <return-response response-variable-name="existing response variable">
          <set-status code="401" reason="Unauthorized" />
          <set-header name="WWW-Authenticate" exists-action="override">
            <value>Bearer error="invalid_token"</value>
          </set-header>
        </return-response>
      </when>
    </choose>

Aan de hand van [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) waarin wordt beschreven hoe `bearer` tokens moeten worden gebruikt, wordt ook retourneren een `WWW-Authenticate` koptekst met het 401 antwoord. WWW-verificatie is bedoeld om te geven dat een client over het samenstellen van een verzoek voor een goed geautoriseerde. Vanwege de grote verscheidenheid aan benaderingen mogelijkheden van het kader OAuth2 is het moeilijk zijn om te communiceren van de benodigde informatie. Er zijn gelukkig inspanningen al bezig om te helpen [clients Ontdek hoe u goed Autoriseer aanvragen voor een resource-server](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).

### <a name="final-solution"></a>Definitief-oplossing
Het samenstellen van alle onderdelen, wordt het volgende beleid ophalen:

    <inbound>
      <!-- Extract Token from Authorization header parameter -->
      <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

      <!-- Send request to Token Server to validate token (see RFC 7662) -->
      <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
        <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
        <set-method>POST</set-method>
        <set-header name="Authorization" exists-action="override">
          <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
        </set-header>
        <set-header name="Content-Type" exists-action="override">
          <value>application/x-www-form-urlencoded</value>
        </set-header>
        <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
      </send-request>

      <choose>
            <!-- Check active property in response -->
            <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
                <!-- Return 401 Unauthorized with http-problem payload -->
                <return-response response-variable-name="existing response variable">
                    <set-status code="401" reason="Unauthorized" />
                    <set-header name="WWW-Authenticate" exists-action="override">
                        <value>Bearer error="invalid_token"</value>
                    </set-header>
                </return-response>
            </when>
        </choose>
      <base />
    </inbound>

Dit is alleen een aantal voorbeelden van hoe de `send-request` beleid handig externe services integreren in het proces van vergaderverzoeken en antwoorden die doorloopt tot en met de API Management-service kan worden gebruikt.

## <a name="response-composition"></a>Antwoord samenstelling
De `send-request` beleid kan worden gebruikt voor het verbeteren van een primaire verzoek om een back endsysteem, zoals in het vorige voorbeeld hebt gezien of deze kan worden gebruikt als een volledige vervangen voor van de backend-oproep. Gebruik deze techniek kunnen we eenvoudig maken samengestelde bronnen die worden samengevoegd uit meerdere andere systemen voor.

### <a name="building-a-dashboard"></a>Samenstellen van een dashboard   
Soms wilt u mogelijk om weer te geven van de gegevens die zich in meerdere backend-systemen, bijvoorbeeld, om een dashboard te sturen. De KPI's afkomstig zijn uit alle andere back-uiteinden, maar u liever niet voor directe toegang tot deze en het is nette als alle gegevens in één aanvraag kan worden opgehaald. Misschien moet enkele van de backend-informatie sommige segmenteren en dobbelstenen en een beetje sanitizing eerst! Wordt het cachegeheugen die samengestelde bron kunnen zou een handige verkleinen van de backend-laden, zoals u weet dat gebruikers hebben een gewoonte van hammering de toets F5 om te zien als de doelstellingen van hun underperforming kunnen worden gewijzigd.    

### <a name="faking-the-resource"></a>De resource faking
De eerste stap tot het bouwen van onze resource dashboard is een nieuwe bewerking configureren in de publisher-API-beheerportal. Dit is een tijdelijke aanduiding voor bewerking gebruikt voor het configureren van ons beleid samenstelling om te maken van onze dynamische bron.

![Dashboard bewerking](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a>Het de aanvragen
Wanneer de `dashboard` bewerking is gemaakt we een beleid specifiek voor de bewerking kunt configureren. 

![Dashboard bewerking](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

De eerste stap is een queryparameters extraheren uit de inkomende aanvraag, zodat we naar onze backend doorschakelen kunt. In dit voorbeeld onze dashboard informatie op basis van een bepaalde periode wordt weergegeven een daarom heeft een `fromDate` en `toDate` parameter. We de beschikking over de `set-variable` beleid voor het uitpakken van de gegevens uit de aanvraag-URL.

    <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
    <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

Nadat u deze informatie kunnen we aanvragen tot alle backend-systemen. Elk verzoek om een nieuwe URL met de parameterinformatie wordt en de desbetreffende server belt en slaat het antwoord in een context-variabele.

    <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
      <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
      <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

Deze verzoeken wordt in volgorde, dat wil niet ideaal zeggen uitgevoerd. In een nieuwe versie we een nieuw beleid genoemd Introductie `wait` waarmee alle volgende aanvragen voor het parallel uitvoeren.

### <a name="responding"></a>Reageert

Als u wilt samenstellen van het samengestelde antwoord kunnen we het beleid [return-antwoord](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) gebruiken. De `set-body` element expressies kunt gebruiken om een nieuwe te `JObject` met alle de component weergaven ingesloten als eigenschappen.

    <return-response response-variable-name="existing response variable">
      <set-status code="200" reason="OK" />
      <set-header name="Content-Type" exists-action="override">
        <value>application/json</value>
      </set-header>
      <set-body>
        @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                      new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                      new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                      new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                      ).ToString())
      </set-body>
    </return-response>

Het volledige beleid ziet er als volgt uit:

    <policies>
        <inbound>

      <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
      <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

        <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
          <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
          <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <return-response response-variable-name="existing response variable">
          <set-status code="200" reason="OK" />
          <set-header name="Content-Type" exists-action="override">
            <value>application/json</value>
          </set-header>
          <set-body>
            @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                          new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                          new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                          new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                          ).ToString())
          </set-body>
        </return-response>
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <base />
        </outbound>
    </policies>

In de configuratie van de tijdelijke aanduiding voor betekent bewerking die we de resource dashboard mogen worden opgeslagen voor ten minste een uur omdat we begrijpen dat de aard van de gegevens kunt configureren dat zelfs als dit een uur verouderd is, dat deze nog steeds niet voldoende effectieve overbrengen nuttige informatie aan de gebruikers.

## <a name="summary"></a>Overzicht
Azure API Management-service biedt flexibele beleidsregels waarmee selectief kunnen worden toegepast op HTTP-verkeer en samenstelling van backend-services kunt. Of u wilt verbeteren van de gateway API met waarschuwingen functies, verificatie, validatie mogelijkheden of maak nieuwe samengestelde resources op basis van meerdere backend-services, de `send-request` en gerelateerde beleidsregels open een wereld van mogelijkheden.

## <a name="watch-a-video-overview-of-these-policies"></a>Bekijk een video-overzicht van dit beleid
Voor meer informatie over de [- een-manier-verzoek om te verzenden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [- verzoek om te verzenden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest)en [return-antwoord](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) beleidsregels beschreven in dit artikel, neemt u de volgende video bekijken.

> [AZURE.VIDEO send-request-and-return-response-policies]
