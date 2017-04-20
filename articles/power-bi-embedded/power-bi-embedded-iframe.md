<properties
   pageTitle="Het gebruik van Power BI ingesloten met REST | Microsoft Azure"
   description="Informatie over het gebruik van Power BI ingesloten met REST "
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="how-to-use-power-bi-embedded-with-rest"></a>Het gebruik van Power BI ingesloten met REST


## <a name="power-bi-embedded-what-it-is-and-what-its-for"></a>Power BI ingesloten: Wat is het en wat is voor
Een overzicht van Power BI ingesloten wordt beschreven in de officiële [site van Power BI ingesloten](https://azure.microsoft.com/services/power-bi-embedded/), maar we even verder kijken snelle voordat we hier op de details over het gebruik van deze met REST.

Het is echt heel eenvoudig. Een ISV wil vaak de dynamische gegevensvisualisaties van [Power BI](https://powerbi.microsoft.com) in hun eigen toepassing als UI-bouwstenen gebruiken.

Maar u weet, Power BI-rapporten of tegels insluiten in uw webpagina is al mogelijk zonder het Power BI ingesloten Azure-service, met behulp van de **Power BI-API**. Als u uw rapporten in uw organisatie dezelfde delen wilt, kunt u de rapporten insluiten met Azure AD-verificatie. De gebruiker van wie de rapporten weergaven moet aanmelden met hun eigen Azure AD-account. Als u wilt delen van uw rapporten voor alle gebruikers (inclusief externe gebruikers), kunt u gewoon insluiten met anonieme toegang.

Maar u ziet, deze eenvoudige insluiten oplossing niet helemaal voldoet aan de behoeften van een toepassing ISV.
De meeste ISV-toepassingen vereist voor het leveren van de gegevens voor hun eigen klanten, niet per se gebruikers in hun eigen organisatie. Bijvoorbeeld als u deel van de service voor zowel bedrijf A en B bedrijf bieden, moeten gebruikers in bedrijf A alleen gegevens zien voor hun eigen bedrijf A. Meerdere pachtadres is dat wil zeggen nodig voor de bezorging.

De toepassing ISV mogelijk ook een eigen verificatiemethoden zoals formulieren auth, eenvoudige auth, etc aanbod.. Vervolgens de insluiten oplossing moet samen te werken met deze bestaande verificatiemethoden veilig. Het is ook nodig zijn voor gebruikers moeten kunnen gebruikmaken van deze toepassingen ISV zonder de extra aanschaffen of licentie van een Power BI-abonnement.

 **Power BI ingesloten** is bedoeld voor exact dit soort ISV scenario's. Dus nu dat we dat in vogelvlucht uit de weg hebben, we gaan op sommige details

U kunt .NET \(C#) of Node.js SDK, aan uw toepassing met Power BI ingesloten eenvoudig te maken. Maar, in dit artikel leggen we over HTTP-mailstroom \(inclusief AuthN) van Power BI zonder SDK's. Informatie over deze stroom, kunt u uw toepassing **met elke programmeertaal**maken en u kunt begrijpt nauw de essentie van Power BI ingesloten.

## <a name="create-power-bi-workspace-collection-and-get-access-key-provisioning"></a>Maak Power BI-werkruimte siteverzameling en get-toegangstoets \(Provisioning)
Power BI ingesloten is een van de Azure services. Alleen website die gebruikmaakt van Azure-Portal is betalen voor gebruikskosten \(per uur gebruikerssessie), en de gebruiker die het rapport bekijkt is niet gepland of zelfs vereisen een Azure-abonnement.
Voordat u begint de ontwikkeling van onze toepassingen, moeten we de **Power BI-werkruimte siteverzameling** maken met behulp van Azure-Portal.

Elke werkruimte van Power BI ingesloten is de werkruimte voor elke klant (tenant) en we veel werkruimten kunt toevoegen aan elke siteverzameling werkruimte. Dezelfde toegangstoets wordt gebruikt in elke werkruimte-siteverzameling. In-effect, het verzamelen van de werkruimte is de rand van de beveiliging voor Power BI ingesloten.

![](media\power-bi-embedded-iframe\create-workspace.png)

Als we klaar bent met het maken van de siteverzameling van de werkruimte, kopieert u de access-toets van Azure-Portal.

![](media\power-bi-embedded-iframe\copy-access-key.png)

> [AZURE.NOTE] We kunnen ook inrichten van de siteverzameling van de werkruimte en toegangstoets via REST API ophalen. Meer informatie raadpleegt u [Power BI Resource Provider API's](https://msdn.microsoft.com/library/azure/mt712306.aspx).

## <a name="create-pbix-file-with-power-bi-desktop"></a>.Pbix-bestand maken met Power BI Desktop
Vervolgens moet we de gegevensverbinding en rapporten worden ingesloten maken.
Voor deze taak, moet u er geen hoeft te programmeren of de code is. We zojuist Power BI Desktop gebruiken.
In dit artikel won't we leest u over de details over het gebruik van Power BI Desktop. Als u nodig hebt hulp bij de hier, raadpleegt u [aan de slag met Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/). In ons voorbeeld gebruiken we zojuist het [Voorbeeld detailhandel-analyse](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).

![](media\power-bi-embedded-iframe\power-bi-desktop-1.png)

## <a name="create-a-power-bi-workspace"></a>Een Power BI-werkruimte maken

Nu dat de inrichting van alle klaar is, laten we aan de slag van een klant-werkruimte maken in de werkruimte verzameling via REST API's. De volgende HTTP bericht aanvragen (REST) wordt de nieuwe werkruimte maken in onze bestaande workspace-siteverzameling. In ons voorbeeld is de naam van de werkruimte verzameling **mypbiapp**.
We instellen sneltoets, die we eerder hebt gekopieerd, net als **AppKey**. Het is een zeer eenvoudige verificatie!

**HTTP-aanvraag**

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-antwoord**

```
HTTP/1.1 201 Created
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces
RequestId: 4220d385-2fb3-406b-8901-4ebe11a5f6da

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/$metadata#workspaces/$entity",
  "workspaceId": "32960a09-6366-4208-a8bb-9e0678cdbb9d",
  "workspaceCollectionName": "mypbiapp"
}
```

De resulterende **workspaceId** wordt gebruikt voor de volgende latere API-oproepen. De toepassing moet deze waarde bewaren.

## <a name="import-pbix-file-into-the-workspace"></a>.Pbix-bestand importeren in de werkruimte
Elke werkruimte één Power BI Desktop-bestand met een gegevensset kunt hosten \(met inbegrip van instellingen voor gegevensbron) en rapporten. We kunnen onze .pbix-bestand importeren naar de werkruimte, zoals wordt weergegeven in de onderstaande code. Zoals u ziet, kunnen we de binaire van .pbix-bestand met MIME multipart in http uploaden.

De uri fragment **32960a09-6366-4208-a8bb-9e0678cdbb9d** is de workspaceId en query parameter **datasetDisplayName** is de gegevenssetnaam te maken. De gemaakte gegevensset bevat alle gegevens gerelateerde onderdelen in .pbix bestand bijvoorbeeld als de geïmporteerde gegevens, de aanwijzer naar de gegevensbron, enzovoort...

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports?datasetDisplayName=mydataset01
Authorization: AppKey MpaUgrTv5e...
Content-Type: multipart/form-data; boundary="A300testx"

--A300testx
Content-Disposition: form-data

{the content (binary) of .pbix file}
--A300testx--
```

Deze taak importeren mogelijk uitvoeren voor de tijd. Als alles compleet is, kan de toepassing de status van de taak-id importeren met vraag. In ons voorbeeld is de import-id **4eec64dd-533b-47c3-a72c-6508ad854659**.

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659?tenantId=myorg
RequestId: 658bd6b4-b68d-4ec3-8818-2a94266dc220

{"id":"4eec64dd-533b-47c3-a72c-6508ad854659"}
```

De volgende vraagt status met deze import-id:

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659
Authorization: AppKey MpaUgrTv5e...
```

Als de taak niet voltooid, wordt het antwoord HTTP mogelijk als volgt:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: 614a13a5-4de7-43e8-83c9-9cd225535136

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Publishing",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "name": "mydataset01"
}
```

Als de taak voltooid is, is het mogelijk dat het HTTP-antwoord meer als volgt:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: eb2c5a85-4d7d-4cc2-b0aa-0bafee4b1606

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Succeeded",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "reports": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://app.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c"
    }
  ],
  "datasets": [
    {
      "id": "458e0451-7215-4029-80b3-9627bf3417b0",
      "name": "mydataset01",
      "tables": [
      ],
      "webUrl": "https://app.powerbi.com/datasets/458e0451-7215-4029-80b3-9627bf3417b0"
    }
  ],
  "name": "mydataset01"
}
```

## <a name="data-source-connectivity-and-multi-tenancy-of-data"></a>Connectiviteit voor gegevensbronnen \(en meerdere pachtadres van gegevens)
Terwijl bijna alle de onderdelen in .pbix bestand in onze workspace zijn geïmporteerd, zijn de referenties voor gegevensbronnen niet. Worden hierdoor wanneer u met de **DirectQuery-modus**, het ingesloten rapport kan niet weergegeven correct. Maar, wanneer u een **modus voor importeren**gebruikt, wordt het rapport op basis van de bestaande geïmporteerde gegevens kunt bekijken. In dat geval moeten we de referentie met de volgende stappen via REST oproepen instellen.

Eerst moeten we de gateway-gegevensbron krijgen. We weten de **id** van de gegevensset de eerder geretourneerde-id.

**HTTP-aanvraag**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.GetBoundGatewayDatasources
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-antwoord**

```
GET HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: 574b0b18-a6fa-46a6-826c-e65840cf6e15

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#gatewayDatasources",
  "value": [
    {
      "id": "5f7ee2e7-4851-44a1-8b75-3eb01309d0ea",
      "gatewayId": "ca17e77f-1b51-429b-b059-6b3e3e9685d1",
      "datasourceType": "Sql",
      "connectionDetails": "{\"server\":\"testserver.database.windows.net\",\"database\":\"testdb01\"}"
    }
  ]
}
```

Met de resulterende gateway-id en de gegevensbron-id \(raadpleegt u de vorige **gatewayId** en **id** in het geretourneerde resultaat), wordt de referentie van deze gegevensbron kan als volgt wijzigen:

**HTTP-aanvraag**

```
PATCH https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/gateways/ca17e77f-1b51-429b-b059-6b3e3e9685d1/datasources/5f7ee2e7-4851-44a1-8b75-3eb01309d0ea
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "credentialType": "Basic",
  "basicCredentials": {
    "username": "demouser",
    "password": "P@ssw0rd"
  }
}
```

**HTTP-antwoord**

```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
RequestId: 0e533c13-266a-4a9d-8718-fdad90391099
```

In productie, kunnen we ook de verschillende verbindingsreeks voor elke werkruimte met REST API instellen. \(Internet Explorer, we kunt scheidt u de database voor elke klanten.)

Als wordt de verbindingsreeks van de gegevensbron via de REST gewijzigd.

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.SetAllConnections
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "connectionString": "data source=testserver02.database.windows.net;initial catalog=testdb02;persist security info=True;encrypt=True;trustservercertificate=False"
}
```

Of we rij-beveiliging op gebruikersniveau kunt gebruiken in Power BI ingesloten en we kunt scheidt u de gegevens voor elke gebruikers in één rapport. As a Result, wordt elke klant-rapport met dezelfde .pbix kunt inrichten \(UI, enz.) en verschillende gegevensbronnen.

> [AZURE.NOTE] Als u de **modus voor importeren** in plaats van de **DirectQuery-modus**gebruikt, is er geen manier modellen via API vernieuwen. En lokale gegevensbronnen via Power BI gateway nog niet wordt ondersteund in Power BI ingesloten. Echter u echt moet gaten houden op de [Power BI-blog](https://powerbi.microsoft.com/blog/) voor nieuw en wat er nieuw in toekomstige versies.

## <a name="authentication-and-hosting-embedding-reports-in-our-web-page"></a>Verificatie- en hostingprovider (insluiten)-rapporten in onze pagina met webonderdelen

In de vorige REST API kunt we de toegangstoets **AppKey** zelf gebruiken als de koptekst autorisatie. Omdat deze oproepen kunnen worden verwerkt aan de serverzijde backend is het veilig.

Maar, wanneer we het rapport in onze webpagina insluiten, dit soort beveiligingsinformatie proces wordt verwerkt met JavaScript \(frontend). Vervolgens moet de waarde van de autorisatie header worden beveiligd. Als onze toegangstoets is ontdekt door een kwaadwillende gebruiker of schadelijke code, kunnen ze belt u met deze toets bewerkingen.

Wanneer we het rapport in onze webpagina insluiten, moeten we het berekende token gebruiken in plaats van toegangstoets **AppKey**. De toepassing moet maken het OAuth Json Web Token \(JWT) die bestaat uit de claims en de berekende digitale handtekening. Zie de volgende afbeelding, is dit JWT OAuth gecodeerde tekenreeks stip lijstscheidingsteken tokens.

![](media\power-bi-embedded-iframe\oauth-jwt.png)

Eerst moeten we de invoerwaarde, die later is ondertekend voorbereiden. Deze waarde wordt de tekenreeks base64 url codering (rfc4648) van de volgende json en deze worden gescheiden door de stip \(.) teken. We wordt later, wordt uitgelegd hoe kom ik aan de lijst-id.

> [AZURE.NOTE] Als we rij niveau beveiliging (RLS) gebruiken met Power BI ingesloten wilt, moeten we ook **gebruikersnaam** en **rollen** opgeven in de claims.

```
{
  "typ":"JWT",
  "alg":"HS256"
}
```

```
{
  "wid":"{workspace id}",
  "rid":"{report id}",
  "wcn":"{workspace collection name}",
  "iss":"PowerBISDK",
  "ver":"0.2.0",
  "aud":"https://analysis.windows.net/powerbi/api",
  "nbf":{start time of token expiration},
  "exp":{end time of token expiration}
}
```

Vervolgens wordt de tekenreeks base64 codering van HMAC moet maken \(de handtekening) met SHA256 algoritme. Deze ondertekend invoerwaarde, is de vorige tekenreeks.

Laatst, combineren we de ingevoerde waarde en handtekening tekenreeks termijn met \(.) teken. De voltooide tekenreeks is het app-token voor het insluiten van het rapport. Zelfs als de app-token is ontdekt door een kwaadwillende gebruiker, kunnen ze niet de oorspronkelijke toegangstoets ophalen. Deze app token verloopt snel.

Hier volgt een voorbeeld PHP voor de volgende stappen uit:

```
<?php
// 1. power bi access key
$accesskey = "MpaUgrTv5e...";

// 2. construct input value
$token1 = "{" .
  "\"typ\":\"JWT\"," .
  "\"alg\":\"HS256\"" .
  "}";
$token2 = "{" .
  "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
  "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
  "\"wcn\":\"mypbiapp\"," . // workspace collection name
  "\"iss\":\"PowerBISDK\"," .
  "\"ver\":\"0.2.0\"," .
  "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
  "\"nbf\":" . date("U") . "," .
  "\"exp\":" . date("U" , strtotime("+1 hour")) .
  "}";
$inputval = rfc4648_base64_encode($token1) .
  "." .
  rfc4648_base64_encode($token2);

// 3. get encoded signature
$hash = hash_hmac("sha256",
    $inputval,
    $accesskey,
    true);
$sig = rfc4648_base64_encode($hash);

// 4. show result (which is the apptoken)
$apptoken = $inputval . "." . $sig;
echo($apptoken);

// helper functions
function rfc4648_base64_encode($arg) {
  $res = $arg;
  $res = base64_encode($res);
  $res = str_replace("/", "_", $res);
  $res = str_replace("+", "-", $res);
  $res = rtrim($res, "=");
  return $res;
}
?>
```

## <a name="finally-embed-the-report-into-the-web-page"></a>Ten slotte het rapport insluiten in de pagina met webonderdelen

Voor het insluiten van onze rapport, moeten we de URL insluiten en **id** met de volgende REST API rapporteren.

**HTTP-aanvraag**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/reports
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-antwoord**

```
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: d4099022-405b-49d3-b3b7-3c60cf675958

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#reports",
  "value": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c",
      "isFromPbix": false
    }
  ]
}
```

We kunt het rapport insluiten in onze WebApp met het vorige app token.
Als we naar het volgende voorbeeldcode kijken, wordt het deel achter de voormalige is hetzelfde als het vorige voorbeeld. Klik in het laatste onderdeel in dit voorbeeld ziet u de **embedUrl** \(raadpleegt u het vorige resultaat) in de iframe, en is de app-token posten in de ingevoegde iframe.

> [AZURE.NOTE] U moet de waarde van de id rapport wijzigen in een eigen. Vanwege een fout in onze inhoudsbeheersysteem, is de iframe-code in het voorbeeld Lees ook letterlijk. De capped tekst uit de tag verwijderen als u kopieert en plakt u deze code steekproef.

```
    <?php
    // 1. power bi access key
    $accesskey = "MpaUgrTv5e...";

    // 2. construct input value
    $token1 = "{" .
      "\"typ\":\"JWT\"," .
      "\"alg\":\"HS256\"" .
      "}";
    $token2 = "{" .
      "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
      "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
      "\"wcn\":\"mypbiapp\"," . // workspace collection name
      "\"iss\":\"PowerBISDK\"," .
      "\"ver\":\"0.2.0\"," .
      "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
      "\"nbf\":" . date("U") . "," .
      "\"exp\":" . date("U" , strtotime("+1 hour")) .
      "}";
    $inputval = rfc4648_base64_encode($token1) .
      "." .
      rfc4648_base64_encode($token2);

    // 3. get encoded signature value
    $hash = hash_hmac("sha256",
        $inputval,
        $accesskey,
        true);
    $sig = rfc4648_base64_encode($hash);

    // 4. get apptoken
    $apptoken = $inputval . "." . $sig;

    // helper functions
    function rfc4648_base64_encode($arg) {
      $res = $arg;
      $res = base64_encode($res);
      $res = str_replace("/", "_", $res);
      $res = str_replace("+", "-", $res);
      $res = rtrim($res, "=");
      return $res;
    }
    ?>
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <title>Test page</title>
      <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
      <button id="btnView">View Report !</button>
      <div id="divView">
        <**REMOVE THIS CAPPED TEXT IF COPIED** iframe id="ifrTile" width="100%" height="400"></iframe>
      </div>
      <script>
        (function () {
          document.getElementById('btnView').onclick = function() {
            var iframe = document.getElementById('ifrTile');
            iframe.src = 'https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c';
            iframe.onload = function() {
              var msgJson = {
                action: "loadReport",
                accessToken: "<?=$apptoken?>",
                height: 500,
                width: 722
              };
              var msgTxt = JSON.stringify(msgJson);
              iframe.contentWindow.postMessage(msgTxt, "*");
            };
          };
        }());
      </script>
    </body>
```

En hier is onze resultaat:

![](media\power-bi-embedded-iframe\view-report.png)

Op dit moment kan bevat Power BI ingesloten alleen het rapport in de iframe. Maar de [Power BI-Blog]()gaten houden. Toekomstige verbeteringen kunt nieuwe clientzijde API's die worden laat ons gegevens in de ingevoegde iframe verzenden, evenals informatie ophalen uit. Interessante items!


## <a name="see-also"></a>Zie ook
- [Verificatie en machtiging in Power BI ingesloten](power-bi-embedded-app-token-flow.md)
