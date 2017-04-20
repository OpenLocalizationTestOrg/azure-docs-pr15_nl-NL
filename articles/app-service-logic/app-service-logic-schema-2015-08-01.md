<properties 
    pageTitle="Nieuwe schema versie 2015-08-01-voorbeeld" 
    description="Meer informatie over het schrijven van de JSON-definitie voor de meest recente versie van de logica apps" 
    authors="stepsic-microsoft-com" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="stepsic"/>
    
# <a name="new-schema-version-2015-08-01-preview"></a>Nieuwe schema versie 2015-08-01-voorbeeld

Het nieuwe schema en API versie voor logica apps heeft een aantal verbeteringen die de betrouwbaarheid verbeteren en eenvoudig te gebruiken met logica apps. Er zijn 4 belangrijke verschillen:

1. Het type van de actie **APIApp** is bijgewerkt naar een nieuwe **APIConnection** actietype.
2. **Herhaal** is gewijzigd in **Foreach**.
3. De **Luisteraar ervan af HTTP** -API-app is niet meer vereist.
4. Onderliggende werkstromen bellen, wordt een nieuw schema gebruikt.

## <a name="1-moving-to-api-connections"></a>1. verplaatsen naar de API-verbindingen

De grootste wijziging is dat u niet meer nodig hebt voor de implementatie van API-apps in uw abonnement Azure-API's gebruiken. Er zijn 2 manieren kunt u API's:
* Beheerde API's
* Uw aangepaste Web API's

Elk van deze is iets anders afgehandeld omdat het beheer en hostingprovider modellen verschillende kunnen. Een voordeel van dit model is dat u bent niet langer beperkt tot resources die zijn geïmplementeerd in uw resourcegroep. 

### <a name="managed-apis"></a>Beheerde API 's

Er zijn een aantal API's die worden beheerd door Microsoft namens, zoals Office 365, Salesforce, Twitter, FTP enzovoort... Sommige van deze beheerde API's kunnen worden gebruikt als-is, zoals Bing vertalen, terwijl de anderen configuratie vereist. Deze configuratie wordt een *verbinding*genoemd.

Wanneer u Office 365 gebruikt, moet u bijvoorbeeld een verbinding met uw Office 365-aanmelding token maken. Deze token worden veilig opgeslagen en vernieuwd zodat uw app logica kunt altijd de Office 365-API bellen. U kunt ook als u verbinden met uw SQL- of FTP-server wilt, moet u een verbinding met de verbindingsreeks maken. 

Binnen de definitie deze acties worden genoemd `APIConnection`. Hier volgt een voorbeeld van een verbinding die oproepen van Office 365 een e-mail te verzenden:

```
{
    "actions": {
        "Send_Email": {
            "type": "ApiConnection",
            "inputs": {
                "host": {
                    "api": {
                        "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
                    },
                    "connection": {
                        "name": "@parameters('$connections')['shared_office365']['connectionId']"
                    }
                },
                "method": "post",
                "body": {
                    "Subject": "Reminder",
                    "Body": "Don't forget!",
                    "To": "me@contoso.com"
                },
                "path": "/Mail"
            }
        }
    }
}
```

Het gedeelte van de invoer die uniek is voor API verbindingen is de `host` object. Dit bestaat uit twee delen: `api` en `connection`.

De `api` de URL van het waar die API beheerde wordt gehost runtime heeft. Ziet u alle van de beschikbare beheerde API's voor u door te bellen `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.

Wanneer u een API gebruikt, mogelijk of mogelijk geen **verbindingsparameters** gedefinieerd. Als dit niet het geval zijn geen **verbinding** is vereist. Als dat zo is, hebt u een verbinding maken. Als u die verbinding dit de naam die u hebt gemaakt en vervolgens u verwijzingen maken naar die in de `connection` object binnen de `host` object. Een verbinding wilt maken in een resourcegroep, bellen:

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

Met de volgende instantie:


```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues" : {
        "accountName" : "{The name of the storage account -- the set of parameters is different for each API}"
    }
  },
  "location" : "{Logic app's location}"
}
```

### <a name="deploying-managed-apis-in-an-azure-resource-manager-template"></a>API's implementeert worden beheerd in een manager-sjabloon van Azure Resource

U kunt een volledige-toepassing maken in een sjabloon ARM zo lang maken als deze niet interactieve aanmelding vereisen. Als er aanmelden moeten, u kunt alles instellen met de sjabloon ARM, maar nog steeds Ga naar de portal als u akkoord gaat de verbindingen. 

```
    "resources": [{
        "apiVersion": "2015-08-01-preview",
        "name": "azureblob",
        "type": "Microsoft.Web/connections",
        "location": "[resourceGroup().location]",
        "properties": {
            "api": {
                "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
            },
            "parameterValues": {
                "accountName": "[parameters('storageAccountName')]",
                "accessKey": "[parameters('storageAccountKey')]"
            }
        }
    }, {
        "type": "Microsoft.Logic/workflows",
        "apiVersion": "2015-08-01-preview",
        "name": "[parameters('logicAppName')]",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[resourceId('Microsoft.Web/connections', 'azureblob')]"
        ],
        "properties": {
            "sku": {
                "name": "[parameters('sku')]",
                "plan": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/',parameters('svcPlanName'))]"
                }
            },
            "definition": {
                "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#",
                "actions": {
                    "Create_file": {
                        "type": "apiconnection",
                        "inputs": {
                            "host": {
                                "api": {
                                    "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                                },
                                "connection": {
                                    "name": "@parameters('$connections')['azureblob']['connectionId']"
                                }
                            },
                            "method": "post",
                            "queries": {
                                "folderPath": "[concat('/',parameters('containerName'))]",
                                "name": "helloworld.txt"
                            },
                            "body": "@decodeDataUri('data:,Hello+world!')",
                            "path": "/datasets/default/files"
                        },
                        "conditions": []
                    }
                },
                "contentVersion": "1.0.0.0",
                "outputs": {},
                "parameters": {
                    "$connections": {
                        "defaultValue": {},
                        "type": "Object"
                    }
                },
                "triggers": {
                    "recurrence": {
                        "type": "Recurrence",
                        "recurrence": {
                            "frequency": "Day",
                            "interval": 1
                        }
                    }
                }
            },
            "parameters": {
                "$connections": {
                    "value": {
                        "azureblob": {
                            "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                            "connectionName": "azureblob",
                            "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                        }

                    }
                }
            }
        }
    }]
```

U kunt zien in dit voorbeeld dat de verbindingen zijn alleen normale resources die in uw resourcegroep wonen. Ze verwijzen naar de managedAPIs die beschikbaar zijn voor u in uw abonnement.

### <a name="your-custom-web-apis"></a>Uw aangepaste Web-API 's

Als u uw eigen API (specifiek, geen Microsoft-beheerde bestanden) gebruikt, moet u de ingebouwde **HTTP** -actie gebruiken om te bellen ze. Als u wilt een ideaal ervaring hebt, moet u een eindpunt swagger laten zien voor uw API. Hiermee schakelt u de ontwerpfunctie van de app logica om weer te geven van de invoer en uitvoer voor uw API. Zonder een swagger, is de designer alleen mogelijk om weer te geven van de invoer en uitvoer als ondoorzichtig JSON-objecten.

Hier volgt een voorbeeld met de nieuwe `metadata.apiDefinitionUrl` eigenschap:
```
{
   "actions": {
        "mycustomAPI": {
            "type": "http",
            "metadata" : {
              "apiDefinitionUrl" : "https://mysite.azurewebsites.net/api/apidef/"  
            },
            "inputs": {
                "uri": "https://mysite.azurewebsites.net/api/getsomedata",
                "method" : "GET"
            }
        }
    }
}
```

Als u uw Web-API op **App Service** host wordt vervolgens deze automatisch weergegeven in de lijst met acties die beschikbaar zijn in de ontwerpfunctie. Als dat niet zo is, moet u rechtstreeks in de URL te plakken. Het eindpunt swagger moet niet-worden geverifieerd om te worden best binnen de logica apps designer (Hoewel u de API zelf met andere methoden worden ondersteund in de Swagger beveiligen mogelijk).

### <a name="using-your-already-deployed-api-apps-with-2015-08-01-preview"></a>Uw al geïmplementeerd API-apps gebruiken met 2015-08-01-voorbeeld

Als u een API-app hebt geïmplementeerd, kunt u deze kunt bellen via het **HTTP** -actie.

Bijvoorbeeld als u Dropbox met een lijst met bestanden, mogelijk hebt u ongeveer zo uit in de definitie van uw **2014-12-01-preview** -schema-versie:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2014-12-01-preview/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
            "defaultValue": "eyJ0eX...wCn90",
            "type": "String",
            "metadata": {
                "token": {
                    "name": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
                }
            }
        }
    },
    "actions": {
        "dropboxconnector": {
            "type": "ApiApp",
            "inputs": {
                "apiVersion": "2015-01-14",
                "host": {
                    "id": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                    "gateway": "https://avdemo.azurewebsites.net"
                },
                "operation": "ListFiles",
                "parameters": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

U kunt de overeenkomstige HTTP actie zoals hieronder (het gedeelte parameters van de logica app definitie blijft ongewijzigd) maken:

```
{
    "actions": {
        "dropboxconnector": {
            "type": "Http",
            "metadata" : {
              "apiDefinitionUrl" : "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
            },
            "inputs": {
                "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
                "method" : "POST",
                "body": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Deze eigenschappen één voor één doorlopen:

| Actie-eigenschap |  Beschrijving |
| --------------- | -----------  |
| `type` | `Http`In plaats van`APIapp` |
| `metadata.apiDefinitionUrl` | Als u deze actie gebruiken in de ontwerpfunctie van de apps logica wilt, wilt u opnemen van het eindpunt metagegevens. Hiermee wordt opgesteld uit:`{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` | Hiermee wordt opgesteld uit:`{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` | Altijd`POST` |
| `inputs.body` | Gelijk aan de parameters van de app api | 
| `inputs.authentication` | Gelijk aan de verificatie van de app api |

Deze methode moet voor alle API app acties werken. Echter, houd er rekening mee dat deze vorige API-apps worden niet meer ondersteund en moet u verplaatsen naar een van de twee andere opties boven (een beheerde API of hostingprovider van uw aangepaste Web-API).

## <a name="2-repeat-renamed-to-foreach"></a>2. herhalen gewijzigd in Foreach

Voor de vorige schemaversie we een groot aantal feedback van klanten ontvangen dat **Herhaal** is duidelijker is en niet correct vastleggen dat het echt was een voor elke lus. We hebben deze daardoor gewijzigd in **Foreach**. Bijvoorbeeld:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "repeat": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{repeatItem()}"
            }
        }
    }
}
```

Zou nu worden geschreven als:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "foreach": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{item()}"
            }
        }
    }
}
```

Eerder de functie `@repeatItem()` is gebruikt om te verwijzen naar het huidige item wordt iteratie. Dit is vereenvoudigd NET `@item()`. 

### <a name="referencing-the-outputs-of-the-foreach"></a>Verwijst naar de uitvoer van de Foreach
Verder vereenvoudigen, wordt de uitvoer van **Foreach** acties worden verpakt in een object **repeatItems**genoemd. Dit betekent, terwijl de uitvoer van de bovenstaande herhalen zijn:

```
{
    "repeatItems": [
        {
            "name": "pingBing",
            "inputs": {
                "uri": "https://www.bing.com/search?q=0",
                "method": "GET"
            },
            "outputs": {
                "headers": { },
                "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
            }
            "status": "Succeeded"
        }
    ]
}
```

Het wordt nu zijn:

```
[
    {
        "name": "pingBing",
        "inputs": {
            "uri": "https://www.bing.com/search?q=0",
            "method": "GET"
        },
        "outputs": {
            "headers": { },
            "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
        }
        "status": "Succeeded"
    }
]
```

Wanneer u verwijst naar deze uitvoer, om de hoofdtekst van de actie moet u doen:

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "repeat" : "@outputs('pingBing').repeatItems",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@repeatItem().outputs.body"
            }
        }
    }
}
```

Nu kunt u doen in plaats daarvan:

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "foreach" : "@outputs('pingBing')",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@item().outputs.body"
            }
        }
    }
}
```

Met deze wijzigingen, de functies `@repeatItem()`, `@repeatBody()` en `@repeatOutputs()` worden verwijderd.

## <a name="3-native-http-listener"></a>3. systeemeigen HTTP luisteraar ervan af 
De luisteraar ervan af HTTP-mogelijkheden zijn nu ingebouwde, zodat u niet meer nodig hebt om te implementeren van een HTTP luisteraar ervan af API-app. Lees meer over [de volledige details voor hoe u uw aanroepbare logica app eindpunt hier](app-service-logic-http-endpoint.md). 

Met deze wijzigingen, de functie `@accessKeys()` wordt verwijderd en is vervangen door de `@listCallbackURL()` functie voor de toepassing van het eindpunt (indien nodig) aan. Daarnaast moet u nu ten minste één trigger in uw app logica nu definiëren. Als u wilt `/run` de werkstroom, moet u beschikken over een van een `manual`, `apiConnectionWebhook` of `httpWebhook` triggers. 

## <a name="4-calling-child-workflows"></a>4. bellen onderliggende werkstromen

Onderliggende werkstromen bellen vereist eerder, gaat u naar deze werkstroom, verschijnen het toegangstoken en die in vervolgens naar de definitie van de logica-app die u wilt bellen van het kind te plakken. Met de nieuwe schemaversie, de logica apps-engine genereert automatisch een SA's tijdens runtime voor de werkstroom onderliggende, wat betekent dat u niet hoeft te geen geheimen plakt in de definitie.  Hier volgt een voorbeeld:

```
"mynestedwf" : {
    "type" : "workflow",
    "inputs" : {
        "host" : {
            "id" : "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName" : "myendpointtrigger"
        },
        "queries" : {
            "extrafield" : "specialValue"
        },
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        },
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        }
    },
    "conditions" : []
}
```

Een tweede verbetering is dat we wordt worden de onderliggende werkstromen volledige toegang geven tot de binnenkomende aanvraag. Dat betekent dat u kunt parameters doorgeven in de sectie *query's* en in het *kop* -object en u kunt de volledige hoofdtekst volledig definiëren.

Ten slotte, zijn er vereiste wijzigingen in de onderliggende werkstroom. Terwijl voordat u alleen een werkstroom voor het onderliggende rechtstreeks bellen kunt; nu moet u een eindpunt trigger definiëren in de werkstroom voor het bovenliggende om te bellen. In het algemeen, betekent dit dat u een trigger van type **handmatig** toevoegen en gebruikt u die in de definitie van de bovenliggende site. Houd er rekening mee dat het `host` eigenschap specifiek heeft een `triggerName`, omdat u moet altijd opgeven welke u aanroept trigger.

## <a name="other-changes"></a>Andere wijzigingen

### <a name="new-queries-property"></a>Nieuwe eigenschap voor query 's
Alle actietypen nu ondersteuning voor een nieuwe invoer **query's**genoemd. Dit is een gestructureerde object in plaats van u dat u moet de tekenreeks met de hand samenstellen.

### <a name="parse-function-renamed"></a>Parse() functie gewijzigd
Terwijl we spoedig meer inhoudstypen wilt toevoegen de `parse()` functie is gewijzigd in `json()`.

## <a name="coming-soon-enterprise-integration-apis"></a>Binnenkort beschikbaar: Enterprise-integratie-API's
Op dit moment, we nog niet hebt beheerd versies van de Enterprise-integratie beschikbare API's (zoals AS2). Deze worden worden binnenkort beschikbaar zoals beschreven in de [Routekaart](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/). In de tussentijd kunt u uw bestaande geïmplementeerd BizTalk APIs via het HTTP-actie, zoals hierboven in "werken met uw al geïmplementeerd API-apps."
