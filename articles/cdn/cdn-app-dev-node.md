<properties
    pageTitle="Aan de slag met de SDK van Azure CDN voor Node.js | Microsoft Azure"
    description="Leer hoe u Node.js toepassingen voor het beheren van Azure CDN schrijven."
    services="cdn"
    documentationCenter="nodejs"
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="casoper"/>

# <a name="get-started-with-azure-cdn-development"></a>Aan de slag met Azure CDN ontwikkeling

> [AZURE.SELECTOR]
- [Node.js](cdn-app-dev-node.md)
- [.NET](cdn-app-dev-net.md)

U kunt de [Azure CDN SDK voor Node.js](https://www.npmjs.com/package/azure-arm-cdn) automatiseren maken en beheren van CDN-profielen en eindpunten.  Deze zelfstudie begeleidt bij het maken van een eenvoudige Node.js console-toepassing, waarop u verscheidene van de beschikbare bewerkingen.  Deze zelfstudie is niet bedoeld om te beschrijven van alle aspecten van de Azure CDN SDK voor Node.js uitgebreid beschreven.

Als u wilt deze zelfstudie hebt voltooid, u moet al [Node.js](http://www.nodejs.org) **4.x.x** of hoger geïnstalleerd en geconfigureerd.  U kunt elke teksteditor die u wilt maken van uw toepassing Node.js gebruiken.  Deze zelfstudie schrijven ik [Visual Studio-Code](https://code.visualstudio.com)die wordt gebruikt.  

> [AZURE.TIP] Het [project uit deze zelfstudie voltooid](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) is beschikbaar voor downloaden op MSDN.

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a>Uw project maken en toevoegen van NPM afhankelijkheden

Nu we hebt gemaakt van een resourcegroep voor onze CDN-profielen en ze onze Azure AD-toepassing toestemming om CDN profielen en eindpunten in deze groep te beheren, we kunt beginnen met het maken van de toepassing.

Maak een map voor uw toepassing.  Uw huidige locatie naar deze map instellen vanuit console met de hulpmiddelen Node.js in uw huidige pad en geïnitialiseerd van uw project door te voeren:
    
    npm init
    
U wordt weergegeven een reeks vragen initialisatie van uw project.  Deze zelfstudie gebruikt voor het **ingangspunt**, *app.js*.  Mijn andere opties in het volgende voorbeeld, kunt u zien.

![NPM initialisatie uitvoer](./media/cdn-app-dev-node/cdn-npm-init.png)

Onze project is nu geïnitialiseerd met een *packages.json* -bestand.  Onze project dient te sommige Azure bibliotheken in de NPM-pakketten gebruiken.  We gebruiken de Azure Client Runtime voor Node.js ([ms-rest-azure) en de bibliotheek van Azure CDN-Client voor Node.js (azure-arm-cd).  Laten we deze toevoegen aan het project als afhankelijkheden.
 
    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

Nadat de pakketten hebt geïnstalleerd, de *package.json* bestand moet er ongeveer uitzien in dit voorbeeld (versie getallen kunnen verschillen):

``` json
{
  "name": "cdn_node",
  "version": "1.0.0",
  "description": "Azure CDN Node.js tutorial project",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Cam Soper",
  "license": "MIT",
  "dependencies": {
    "azure-arm-cdn": "^0.2.1",
    "ms-rest-azure": "^1.14.4"
  }
}
```

Ten slotte met uw teksteditor, maak een lege tekstbestand en sla deze op in de hoofdmap van onze projectmap als *app.js*.  We kunnen nu klaar om te beginnen met het schrijven van code.

## <a name="requires-constants-authentication-and-structure"></a>Vereist, constanten, verificatie en structuur

Met *app.js* in onze editor is geopend, kunt u de basisstructuur van ons programma voor geschreven we gaan.

1. Toevoegen aan de 'vereist' voor onze NPM-pakketten aan het begin met het volgende:

    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```

2. Moeten we enkele constanten die onze methoden wordt gebruikt definiëren.  Voer de volgende.  Zorg ervoor dat u voor het vervangen van de tijdelijke aanduidingen, inclusief de ** &lt;punthaken&gt;**, met uw eigen waarden naar wens.

    ``` javascript
    //Tenant app constants
    const clientId = "<YOUR CLIENT ID>";
    const clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    const tenantId = "<YOUR TENANT ID>";

    //Application constants
    const subscriptionId = "<YOUR SUBSCRIPTION ID>";
    const resourceGroupName = "CdnConsoleTutorial";
    const resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. Vervolgens we exemplaar van de CDN-client voor management en hieraan onze referenties.

    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
    
    Als u afzonderlijke gebruikersverificatie gebruikt, is deze twee regels er enigszins anders.

    >[AZURE.IMPORTANT] In dit voorbeeld wordt alleen gebruiken als u ervoor kiest om afzonderlijke gebruikersverificatie in plaats van een service principal.  Zorg ervoor dat uw referenties individuele gebruiker bewaken en geheime kunt houden.

    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```

    Zorg ervoor dat u voor het vervangen van de items in ** &lt;punthaken&gt; ** met de juiste gegevens.  Voor `<redirect URI>`, gebruikt u de omleiding URI ingevoerde wanneer u de toepassing geregistreerd in Azure AD.
    

4.  Onze Node.js console-toepassing dient te maken van sommige opdrachtregelparameters.  Laten we valideren ten minste één parameter is doorgegeven.

    ```javascript
    //Collect command-line parameters
    var parms = process.argv.slice(2);

    //Do we have parameters?
    if(parms == null || parms.length == 0)
    {
        console.log("Not enough parameters!");
        console.log("Valid commands are list, delete, create, and purge.");
        process.exit(1);
    }
    ```

5. Waarmee u meer ons het hoofddeel van ons programma, waarbij we vertakken uit andere functies op basis van welke parameters zijn doorgegeven.

    ```javascript
    switch(parms[0].toLowerCase())
    {
        case "list":
            cdnList();
            break;

        case "create":
            cdnCreate();
            break;
        
        case "delete":
            cdnDelete();
            break;

        case "purge":
            cdnPurge();
            break;

        default:
            console.log("Valid commands are list, delete, create, and purge.");
            process.exit(1);
    }
    ```

6.  Op verschillende plaatsen in ons programma moeten we Zorg ervoor dat het juiste aantal parameters zijn doorgegeven en hulp bij de weer te geven als ze niet bevallen.  Laten we maken functies om dat te doen.

    ```javascript
    function requireParms(parmCount) {
        if(parms.length < parmCount) {
            usageHelp(parms[0].toLowerCase());
            process.exit(1);
        }
    }

    function usageHelp(cmd) {
        console.log("Usage for " + cmd + ":");
        switch(cmd)
        {
            case "list":
                console.log("list profiles");
                console.log("list endpoints <profile name>");
                break;

            case "create":
                console.log("create profile <profile name>");
                console.log("create endpoint <profile name> <endpoint name> <origin hostname>");
                break;
            
            case "delete":
                console.log("delete profile <profile name>");
                console.log("delete endpoint <profile name> <endpoint name>");
                break;

            case "purge":
                console.log("purge <profile name> <endpoint name> <path>");
                break;

            default:
                console.log("Invalid command.");
        }
    }
    ```

7. Tot slot worden de functies die we op de client van de management CDN gebruikt voortaan zijn asynchroon, zodat zij nodig hebben een methode om te bellen wanneer ze klaar bent.  We maken dat kan weergeven van de uitvoer van de CDN management-client (indien aanwezig) en sluit u het programma zonder problemen.

    ```javascript
    function callback(err, result, request, response) {
        if (err) {
            console.log(err);
            process.exit(1);
        } else {
            console.log((result == null) ? "Done!" : result);
            process.exit(0);
        }
    }
    ```

Nu de basisstructuur van ons programma is geschreven, moeten we de functies genoemd op basis van onze parameters maken.

## <a name="list-cdn-profiles-and-endpoints"></a>Lijst CDN-profielen en eindpunten

Laten we beginnen met code voor een overzicht van onze bestaande profielen en de eindpunten.  Mijn codeopmerkingen bieden de syntaxis van de verwachte, zodat we weten waar elke parameter hoort.

```javascript
// list profiles
// list endpoints <profile name>
function cdnList(){
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profiles":
            console.log("Listing profiles...");
            cdnClient.profiles.listByResourceGroup(resourceGroupName, callback);
            break;

        case "endpoints":
            requireParms(3);
            console.log("Listing endpoints...");
            cdnClient.endpoints.listByProfile(parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>CDN-profielen en eindpunten maken

Vervolgens gaat we de functies als u wilt maken van profielen en eindpunten schrijven.

```javascript
function cdnCreate() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profile":
            cdnCreateProfile();
            break;

        case "endpoint":
            cdnCreateEndpoint();
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}

// create profile <profile name>
function cdnCreateProfile() {
    requireParms(3);
    console.log("Creating profile...");
    var standardCreateParameters = {
        location: resourceLocation,
        sku: {
            name: 'Standard_Verizon'
        }
    };

    cdnClient.profiles.create(parms[2], standardCreateParameters, resourceGroupName, callback);
}

// create endpoint <profile name> <endpoint name> <origin hostname>        
function cdnCreateEndpoint() {
    requireParms(5);
    console.log("Creating endpoint...");
    var endpointProperties = {
        location: resourceLocation,
        origins: [{
            name: parms[4],
            hostName: parms[4]
        }]
    };

    cdnClient.endpoints.create(parms[3], endpointProperties, parms[2], resourceGroupName, callback);
}
```

## <a name="purge-an-endpoint"></a>Een eindpunt verwijderen

Als dat het eindpunt is gemaakt, is een algemene taak die we mogelijk wilt uitvoeren in ons programma voor inhoud in onze eindpunt verwijderen.

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a>CDN-profielen en eindpunten verwijderen

De laatste functie we nemen Hiermee verwijdert u eindpunten en profielen.

```javascript
function cdnDelete() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        // delete profile <profile name>
        case "profile":
            requireParms(3);
            console.log("Deleting profile...");
            cdnClient.profiles.deleteIfExists(parms[2], resourceGroupName, callback);
            break;

        // delete endpoint <profile name> <endpoint name>
        case "endpoint":
            requireParms(4);
            console.log("Deleting endpoint...");
            cdnClient.endpoints.deleteIfExists(parms[3], parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="running-the-program"></a>Het programma

We kunnen nu onze Node.js-programma met onze favoriete foutopsporing uitvoeren of op de console.

> [AZURE.TIP] Als u Visual Studio-Code als uw foutopsporing gebruikt, moet u uw omgeving instellen om door te geven in de opdrachtregelparameters.  Visual Studio-Code doet dit in het bestand **lanuch.json** .  Zoeken naar een eigenschap **argumenten** met de naam en het toevoegen van een matrix van tekenreekswaarden voor uw parameters, zodat er ongeveer als volgt: `"args": ["list", "profiles"]`.

Laten we beginnen met een overzicht van onze profielen.

![Lijst-profielen](./media/cdn-app-dev-node/cdn-list-profiles.png)

We hij terugkomt een lege matrix.  Aangezien er geen profielen in onze resourcegroep, die wordt verwacht.  Laten we nu een profiel maken.

![Profiel maken](./media/cdn-app-dev-node/cdn-create-profile.png)

Nu kunnen we een eindpunt toevoegen.

![Eindpunt maken](./media/cdn-app-dev-node/cdn-create-endpoint.png)

Tot slot laten we onze profiel verwijderen.

![Profiel verwijderen](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a>Volgende stappen

U wilt zien van de voltooide project van deze procedure, [downloaden van de steekproef](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).

Bekijk de [verwijzing](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/)om te zien de verwijzing voor de SDK van Azure CDN voor Node.js.

Bekijk de [volledige verwijzing](http://azure.github.io/azure-sdk-for-node/)aanvullende documentatie over de SDK Azure voor Node.js vindt.

Het beheren van uw resources CDN met [PowerShell](./cdn-manage-powershell.md).

