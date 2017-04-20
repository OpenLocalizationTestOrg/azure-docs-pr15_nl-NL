<properties
    pageTitle="Azure naslaginformatie voor ontwikkelaars van functies | Microsoft Azure"
    description="Concepten van Azure-functies en de onderdelen die voor alle talen en bindingen gelden te begrijpen."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure-functies, functies, verwerking van de gebeurtenis, webhooks, dynamische berekeningscluster, als u kiest architectuur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-developer-reference"></a>Azure naslaginformatie voor ontwikkelaars van functies

Azure functies delen een paar core technische concepten en de onderdelen, ongeacht de taal of binding die u gebruikt. Voordat u gaan in de details die specifiek zijn voor een bepaalde taal of binding leren, moet u dit overzicht die van toepassing op alle labels doorlezen.

In dit artikel wordt ervan uitgegaan dat u het [overzicht van functies van Azure](functions-overview.md) al hebt gelezen en bekend met [WebJobs SDK concepten zoals triggers, bindingen, en de runtime JobHost bent](../app-service-web/websites-dotnet-webjobs-sdk.md). Azure functies is gebaseerd op de WebJobs SDK. 


## <a name="function-code"></a>Functiecode

Een *functie* is het primaire concept in Azure-functies. U schrijft code voor een functie in een taal van uw keuze en de code-bestanden en een configuratiebestand opslaan in dezelfde map. Configuratie in JSON wordt weergegeven en het bestand de naam `function.json`. Verschillende talen worden ondersteund en elkaar heeft een iets anders ervaring die is geoptimaliseerd voor de meest geschikt zijn voor die taal. 

De `function.json` bestand configuratie die specifiek zijn voor een functie, inclusief de bindingen bevat. De runtime leest dit bestand om te bepalen welke gebeurtenissen worden activeren uitschakelen van, van de functie zelf welke gegevens u wilt opnemen bij het aanroepen van de functie en waar u om gegevens te verzenden doorgegeven. 

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

U kunt voorkomen dat de runtime uitgevoerd van de functie door in te stellen de `disabled` eigenschap `true`.

De `bindings` eigenschap is waar u zowel triggers en bindingen configureren. Elke binding deelt een paar algemene instellingen en enkele instellingen die specifiek voor een bepaald type binding zijn. Elke binding is vereist voor de volgende instellingen:

|Eigenschap|Waarden/typen|Opmerkingen|
|---|-----|------|
|`type`|tekenreeks|Type binding. Bijvoorbeeld `queueTrigger`.
|`direction`|'in', 'uitzoomen'| Geeft aan of de binding voor het ontvangen van gegevens in de functie of verzenden van gegevens van de functie.
| `name` | tekenreeks | De naam die wordt gebruikt voor de afhankelijke gegevens in de functie. Voor C# is dit de naam van een argument; voor JavaScript, worden de sleutel in de lijst met een toets/waarden.

## <a name="function-app"></a>Functie-app

Een app functie bestaat uit een of meer afzonderlijke functies die bij elkaar worden beheerd door Azure App-Service. Alle functies in een app functie delen dezelfde prijzen plan, continue implementatie en runtime-versie. Functies die worden geschreven in meerdere talen kunnen alle de dezelfde functie app delen. Als het ware een functie-app een manier te ordenen en de functies voor gezamenlijk te beheren. 

## <a name="runtime-script-host-and-web-host"></a>Runtime (scripthost en webhost)

De runtime of scripthost, is het onderliggende WebJobs SDK host luistert voor gebeurtenissen, worden verzameld en verzonden gegevens en uiteindelijk wordt uitgevoerd van uw code. 

Om te vergemakkelijken HTTP triggers, is er ook een webhost die u moet gaan zitten vóór de scripthost in productie scenario's is ontworpen. Hierdoor isoleren de scripthost van de front-end van verkeer beheerd door de webhost.

## <a name="folder-structure"></a>Mapstructuur

[AZURE.INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Bij het instellen van een project voor het implementeren van functies in een functie-app in Azure App-Service, kunt u deze mapstructuur behandelen als de sitecode van uw. U kunt bestaande hulpprogramma's zoals doorlopend integratie en implementatie of aangepaste implementatie scripts hiervoor tijd pakketinstallatie implementeren of foutcode transpilation.

>[AZURE.NOTE] De `wwwroot` map hier is waar uw bestanden naar wordt geïmplementeerd. Echter niet vergeet deze map in de bestanden die u implementeert, waardoor uiteindelijk een met `wwwroot\wwwroot`. In plaats daarvan uw `host.json` bestands- en functie mappen moet rechtstreeks in de hoofdmap van wat u implementeren.

## <a id="fileupdate"></a>Het bijwerken van de functie app-bestanden

De functie editor in de portal van Azure ingebouwd kunt u het bestand *function.json* en de codebestand voor een functie bijwerken. Als u wilt uploaden of andere bestanden zoals *package.json* of *project.json* of afhankelijkheden bijwerkt, moet u andere implementatiemethoden gebruiken.

Functie apps zijn gebaseerd op App-Service, zodat alle [distributieopties beschikbaar tot standaard WebApps](../app-service-web/web-sites-deploy.md) beschikbaar voor de functie apps ook zijn. Hier volgen enkele methoden die u gebruiken kunt om te uploaden of bijwerken van de functie app-bestanden. 

#### <a name="to-use-app-service-editor"></a>App Service Editor gebruiken

1. Klik op **instellingen van de functie-app**in de portal Azure-functies.

2. Klik op **Ga naar de App-Service-instellingen**in het gedeelte **Geavanceerde instellingen** .

3. Klik op **App Service Editor** in de App-Menu van het navigatiedeelvenster onder **Hulpmiddelen voor de ontwikkeling**.

4.  Klik op **Zoeken**.

    Nadat de App Service Editor wordt geladen, ziet u de *host.json* bestands- en functie mappen onder *wwwroot*. 

5. Open bestanden bewerken, of slepen en neerzetten van uw computer ontwikkeling om bestanden te uploaden.

#### <a name="to-use-the-function-apps-scm-kudu-endpoint"></a>Gebruik van de functie-app SCM (Kudu) eindpunt

1. Ga naar: `https://<function_app_name>.scm.azurewebsites.net`.

2. Klik op **Console Foutopsporing > CMD**.

3. Navigeer naar `D:\home\site\wwwroot\` *host.json* bijwerken of `D:\home\site\wwwroot\<function_name>` van een functie bestanden wilt bijwerken.

4. Slepen en neerzetten een bestand dat u wilt uploaden in de juiste map in het raster bestand. Er zijn twee gebieden in het bestand raster waar u een bestand kunt neerzetten. Voor *zip-* bestanden ziet u een vak met het label "Sleep hier om te uploaden en pak." Voor andere bestandstypen, zet u in het raster bestand maar buiten het vak "Pak".

#### <a name="to-use-ftp"></a>Gebruik FTP-

1. Volg de instructies [hier](../app-service-web/web-sites-deploy.md#ftp) om FTP geconfigureerd.

2. Wanneer u met de functie app-site verbonden bent, kopieert u een bijgewerkte *host.json* -bestand om te `/site/wwwroot` of functie bestanden kopiëren naar `/site/wwwroot/<function_name>`.

#### <a name="to-use-continuous-deployment"></a>Gebruik van continue implementatie

Volg de instructies in het onderwerp [continue implementatie van Azure-functies](functions-continuous-deployment.md).

## <a name="parallel-execution"></a>Parallelle uitvoering

Wanneer meerdere die als trigger dient gebeurtenissen sneller plaatsvinden dan een runtime single thread-functie kan ze verwerken, kan de runtime de functie meerdere keren parallel roepen.  Als een functie-app wordt gebruikt het [Dynamische Service plannen](functions-scale.md#dynamic-service-plan), kan de functie app schaal automatisch.  Elk exemplaar van de functie-app of de app wordt uitgevoerd op het dynamische Service-Plan of een gewone [App-Service plannen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), mogelijk gelijktijdige functie aanroepen parallel met meerdere threads verwerken.  Het maximum aantal gelijktijdige functie aanroepen in elk exemplaar van de app functie afhankelijk van de grootte van het geheugen van de functie-app. 

## <a name="azure-functions-pulse"></a>Azure functies Pulse  

Pulse is een live-gebeurtenis stream waarin wordt getoond hoe vaak uw functie wordt uitgevoerd, alsmede geslaagde en mislukte. U kunt ook uw gemiddelde tijd voor uitvoeren controleren. Worden er nog meer functies en aanpassingen ernaar na verloop van tijd. U kunt de pagina **Pulse** openen vanaf het tabblad **controle** .

## <a name="repositories"></a>Opslagplaatsen

De code voor functies van Azure bron openen en die zijn opgeslagen in GitHub opslagplaatsen:

* [Azure functies runtime](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Azure functies-portal](https://github.com/projectkudu/AzureFunctionsPortal)
* [Azure functies-sjablonen](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/)
* [Azure WebJobs SDK extensies](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Bindingen

Hier volgt een tabel met alle ondersteunde bindingen.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

## <a name="reporting-issues"></a>Problemen rapporteren

[AZURE.INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)] 

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

* [Azure functies C# naslaginformatie voor ontwikkelaars](functions-reference-csharp.md)
* [Azure functies F # naslaginformatie voor ontwikkelaars](functions-reference-fsharp.md)
* [Azure functies NodeJS-naslaginformatie voor ontwikkelaars](functions-reference-node.md)
* [Azure functies triggers en bindingen](functions-triggers-bindings.md)
* [Azure-functies: de reis](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) op het teamblog Azure App-Service. Een geschiedenis van hoe de functies van Azure is ontwikkeld.





