<properties
    pageTitle="Aangepaste caching in Azure API Management"
    description="Leer hoe u items in de cache door sleutel in Azure API Management"
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

# <a name="custom-caching-in-azure-api-management"></a>Aangepaste caching in Azure API Management
Azure API-beheerservice heeft ingebouwde ondersteuning voor [caching van HTTP-antwoord](api-management-howto-cache.md) van de resource-URL als de sleutel gebruiken. De toets kan worden gewijzigd door verzoek kopteksten met de `vary-by` eigenschappen. Dit is handig voor het hele HTTP-antwoorden (of weergaven) caching, maar soms is het handig om alleen cache een gedeelte van een weergave. Het nieuwe [cache zoekwaarde](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) en de [cache-store-waarde](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) beleid bieden de mogelijkheid om te slaan en willekeurige stukjes gegevens vanuit beleidsdefinities op te halen. Deze mogelijkheid ook waarde wordt toegevoegd aan het beleid eerder geïntroduceerd [verzoek om te verzenden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) omdat u kunt nu cache antwoorden van externe services.

## <a name="architecture"></a>Architectuur  
API-beheerservice gebruikt een gegevenscache gedeelde per tenant zodat, zoals u maximaal schalen meerdere eenheden krijgt u nog steeds toegang tot dezelfde in cache gegevens opgeslagen. Wanneer u werkt met een meerdere landen/regio-implementatie zijn er echter onafhankelijke cache binnen elke regio. Vanwege dit is het belangrijk dat de cache niet als een gegevensopslag, waar de enige bron van een blok informatie is behandelen. Als u hebt en later besloten om te profiteren van de implementatie van meerdere landen/regio, klikt u vervolgens klanten met gebruikers die gaan geen toegang meer tot die in de cache opgeslagen gegevens.

## <a name="fragment-caching"></a>Fragment caching
Er zijn bepaalde gevallen waarin antwoorden worden geretourneerd bevatten een gedeelte van de gegevens die is dure om te bepalen en voor een redelijk tijdsbestek nog up-to-date blijft. Een voorbeeld, kunt u beter een service die is gemaakt door een luchtvaartmaatschappij vindt u informatie over Vliegtickets boeken, flight status, enzovoort. Als de gebruiker een lid van het programma dat airlines wordt verwezen is, moet ze ook informatie met betrekking tot hun huidige status en reiskosten loop. Deze gebruiker-gerelateerde gegevens in een andere mogelijk zijn opgeslagen, maar deze mogelijk wenselijk om deze opnemen in antwoorden over flight status- en apparatuurreserveringen geretourneerd. U kunt dit doen via een proces genaamd caching van fragment. De primaire voorstelling kan worden opgehaald van de originele-server met een soort token om aan te geven waar de gegevens betrekking hebben van een gebruiker moet worden ingevoegd. 

Houd rekening met het volgende JSON-antwoord van een end-API.


    {
      "airline" : "Air Canada",
      "flightno" : "871",
      "status" : "ontime",
      "gate" : "B40",
      "terminal" : "2A",
      "userprofile" : "$userprofile$"
    }  

En secundaire bron aan `/userprofile/{userid}` dat eruitziet

     { "username" : "Bob Smith", "Status" : "Gold" }

Om te bepalen de gewenste gebruikersgegevens om op te nemen, moeten we identificeren die de eindgebruiker is. Deze methode is afhankelijke-implementatie. Als u bijvoorbeeld ik gebruik de `Subject` claimen van een `JWT` token. 

    <set-variable
      name="enduserid"
      value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

We dit opslaan `enduserid` waarde in een variabele context voor later gebruik. De volgende stap is om te bepalen als een eerdere aanvraag al gegevens van de gebruiker opgehaald en in de cache opgeslagen. Hiervoor gebruiken we de `cache-lookup-value` beleid.

      <cache-lookup-value
      key="@("userprofile-" + context.Variables["enduserid"])"
      variable-name="userprofile" />

Als er geen vermelding in de cache die met de sleutelwaarde, dan Nee overeenkomt `userprofile` context variabele wordt gemaakt. We controleren of het succes van het gebruik van de zoekactie de `choose` beleid overdrachten bepalen.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("userprofile"))">
            <!— If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
        </when>
    </choose>


Als de `userprofile` context variabele niet bestaat, en vervolgens gaan we moet maken van een HTTP-aanvraag om op te halen.

    <send-request
      mode="new"
      response-variable-name="userprofileresponse"
      timeout="10"
      ignore-error="true">

      <!-- Build a URL that points to the profile for the current end-user -->
      <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
          (string)context.Variables["enduserid"]).AbsoluteUri)
      </set-url>
      <set-method>GET</set-method>
    </send-request>

We gebruiken de `enduserid` om de URL voor de resource van gebruikersprofiel te maken. Nadat we het antwoord hebt, kunnen we de hoofdtekst van het antwoord halen en sla deze weer in een variabele context.

    <set-variable
        name="userprofile"
        value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

Om te voorkomen ons met deze HTTP-aanvraag om weer te maken, wanneer de dezelfde gebruiker een nieuwe aanvraag maakt, kunnen we het gebruikersprofiel worden opgeslagen in de cache.

    <cache-store-value
        key="@("userprofile-" + context.Variables["enduserid"])"
        value="@((string)context.Variables["userprofile"])" duration="100000" />

We opslaan de waarde in de cache met de exacte dezelfde toets die we oorspronkelijk heeft geprobeerd om op te halen met. De duur die we kiezen voor de opslag van de waarde moet worden gebaseerd op hoe vaak de wijzigingen van de informatie en hoe fouttolerante gebruikers tot verouderd informatie zijn. 

Het is belangrijk Houd er rekening mee dat ophalen uit de cache nog steeds een out-van-process, netwerkaanvraag is en mogelijk nog steeds Voeg tientallen milliseconden toe aan het verzoek. De voordelen afkomstig zijn bij het bepalen van dat de gebruikersprofielinformatie aanzienlijk langer duurt dan die vanwege hoeft te database query's of statistische gegevens uit meerdere back-einden.

De laatste stap in het proces is het geretourneerde antwoord met onze gebruikersprofielinformatie bijwerken.

    <!—Update response body with user profile-->
    <find-and-replace
        from='"$userprofile$"'
        to="@((string)context.Variables["userprofile"])" />

Ik heb gekozen de aanhalingstekens opnemen als onderdeel van het token zodat zelfs wanneer de vervangen wordt niet uitgevoerd, het antwoord JSON nog geldig is. Dit was hoofdzakelijk zodat foutopsporing gemakkelijker.

Nadat u alle volgende stappen uit elkaar combineren, is het eindresultaat een beleid die op de volgende fase lijkt.

     <policies>
        <inbound>
            <!-- How you determine user identity is application dependent -->
            <set-variable
              name="enduserid"
              value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

            <!--Look for userprofile for this user in the cache -->
            <cache-lookup-value
              key="@("userprofile-" + context.Variables["enduserid"])"
              variable-name="userprofile" />

            <!-- If we don’t find it in the cache, make a request for it and store it -->
            <choose>
                <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                    <!—Make HTTP request to get user profile -->
                    <send-request
                      mode="new"
                      response-variable-name="userprofileresponse"
                      timeout="10"
                      ignore-error="true">

                       <!-- Build a URL that points to the profile for the current end-user -->
                        <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),(string)context.Variables["enduserid"]).AbsoluteUri)</set-url>
                        <set-method>GET</set-method>
                    </send-request>

                    <!—Store response body in context variable -->
                    <set-variable
                      name="userprofile"
                      value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

                    <!—Store result in cache -->
                    <cache-store-value
                      key="@("userprofile-" + context.Variables["enduserid"])"
                      value="@((string)context.Variables["userprofile"])"
                      duration="100000" />
                </when>
            </choose>
            <base />
        </inbound>
        <outbound>
            <!—Update response body with user profile-->
            <find-and-replace
                  from='"$userprofile$"'
                  to="@((string)context.Variables["userprofile"])" />
            <base />
        </outbound>
     </policies>

Deze methode in de cache wordt vooral gebruikt in websites waar HTML bestaat op de server zodat deze kan worden weergegeven als één pagina. Kan het echter ook zijn handig in API's waar clients is niet mogelijk client zijde caching van HTTP of het wenselijk niet moeten worden geplaatst die verantwoordelijk op de client is.

Dit hetzelfde soort fragment caching is ook mogelijk op de backend-endwebservers met een bestand Vgx. caching van server, echter de API Management-service deze inzetten kan nuttig zijn bij het in de cache fragmenten afkomstig zijn uit verschillende back-uiteinden dan de primaire antwoorden.

## <a name="transparent-versioning"></a>Transparante versiebeheer
Het is gebruikelijk voor meerdere versies van de andere implementatie van een API ondersteund op elk gewenst moment. Dit is misschien ter ondersteuning van verschillende omgevingen, zoals ontwikkelaar, test, productie, enzovoort, of is het mogelijk ter ondersteuning van oudere versies van de API om tijd voor consumenten API om te migreren naar de nieuwere versies te geven. 

Een benadering van dit in plaats van het vereisen van client ontwikkelaars voor het wijzigen van de URL's uit voor de verwerking `/v1/customers` naar `/v2/customers` opslaan in de profielgegevens van de consument welke versie van de API ze momenteel wilt gebruiken en de juiste backend-URL bellen. Om te bepalen de juiste backend-URL voor een bepaalde client bellen, is het nodig zijn om enkele configuratiegegevens query's mogelijk. Door deze configuratiegegevens caching, kunt we de prestaties van deze zoekactie manieren minimaliseren.

De eerste stap is om te bepalen de id die wordt gebruikt voor het configureren van de gewenste versie. Ik heb gekozen in dit voorbeeld om te koppelen van de versie moet de productcode voor het abonnement. 

        <set-variable name="clientid" value="@(context.Subscription.Key)" />

Dan doen we een opzoekactie cache om te zien als we al hebt opgehaald de gewenste clientversie.

        <cache-lookup-value
        key="@("clientversion-" + context.Variables["clientid"])"
        variable-name="clientversion" />

Vervolgens controleren we om te zien als we niet in de cache vinden hebt.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("clientversion"))">

Als we niet en we gaan en deze op te halen.

    <send-request
        mode="new"
        response-variable-name="clientconfiguresponse"
        timeout="10"
        ignore-error="true">
                <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                <set-method>GET</set-method>
    </send-request>

De hoofdtekst van het antwoord ophalen uit het antwoord.

    <set-variable
          name="clientversion"
          value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />

Sla het weer in de cache voor toekomstig gebruik.

    <cache-store-value
          key="@("clientversion-" + context.Variables["clientid"])"
          value="@((string)context.Variables["clientversion"])"
          duration="100000" />

En tot slot werkt u de URL van de back-enddatabase om te selecteren van de versie van de service de keuze van de client.

    <set-backend-service
          base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />

Het beleid volledig wordt als volgt.

     <inbound>
        <base />
        <set-variable name="clientid" value="@(context.Subscription.Key)" />
        <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

        <!-- If we don’t find it in the cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("clientversion"))">
                <send-request mode="new" response-variable-name="clientconfiguresponse" timeout="10" ignore-error="true">
                    <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                    <set-method>GET</set-method>
                </send-request>
                <!-- Store response body in context variable -->
                <set-variable name="clientversion" value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
                <!-- Store result in cache -->
                <cache-store-value key="@("clientversion-" + context.Variables["clientid"])" value="@((string)context.Variables["clientversion"])" duration="100000" />
            </when>
        </choose>
        <set-backend-service base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
    </inbound>


Zodat consumenten API transparant bepalen welke versie u backend door clients wordt geopend zonder dat u moet bijwerken en implementeer deze opnieuw clients is een elegante oplossing die voldoet aan veel API versiebeheer bezwaren.

## <a name="tenant-isolation"></a>Tenant moeten worden geïsoleerd

Sommige bedrijven Maak in grotere, meerdere tenant implementaties afzonderlijke groepen tenants op afzonderlijke implementaties van backend hardware. Hiermee wordt het aantal klanten die worden beïnvloed door de hardware op de backend beperkt. Bovendien kunt nieuwe softwareversies om te worden geïmplementeerd in fasen. Als deze backend-architectuur worden transparant voor consumenten API. Dit kan worden bereikt op een soortgelijke manier in transparante versiebeheer omdat deze is gebaseerd op dezelfde methode van het bewerken van de backend-URL met de status van de configuratie per API-sleutel.  

In plaats van een voorkeur versie van de API voor elke sleutel abonnement wordt geretourneerd, zou u een id die betrekking een tenant aan de Hardwaregroep toegewezen hebben retourneren. Deze id kan worden gebruikt om de juiste backend-URL te maken.

## <a name="summary"></a>Overzicht
Het vrij de cache voor het beheer van Azure-API gebruiken voor het opslaan van elk type gegevens efficiënt toegang biedt tot configuratiegegevens die van invloed kunnen zijn op de manier waarop die een binnenkomende aanvraag wordt verwerkt. Dit kan ook worden gebruikt voor de opslag van gegevens fragmenten die antwoorden, als resultaat gegeven van een end-API kunnen verbeteren.

## <a name="next-steps"></a>Volgende stappen
Geef ons uw feedback in de thread Disqus voor dit onderwerp als er andere scenario's die dit beleid hebt ingeschakeld voor u, of als er scenario's u wilt bereiken echter niet zo te zijn momenteel mogelijk.
