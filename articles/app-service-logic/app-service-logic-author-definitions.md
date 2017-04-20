<properties 
    pageTitle="Logica App definities van de auteur | Microsoft Azure" 
    description="Meer informatie over het schrijven van de JSON-definitie voor logica-apps" 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jehollan"/>
    
# <a name="author-logic-app-definitions"></a>Auteur logica App definities
Dit onderwerp wordt beschreven hoe u definities van [Azure logica Apps](app-service-logic-what-are-logic-apps.md) , dat wil een eenvoudige, declaratieve JSON-taal zeggen. Als u dit nog niet hebt gedaan nog, raadpleegt u [hoe u een nieuwe logica-app maakt](app-service-logic-create-a-logic-app.md) eerst. U kunt ook het [volledige referentiemateriaal van de definition language op MSDN](http://aka.ms/logicappsdocs)lezen.

## <a name="several-steps-that-repeat-over-a-list"></a>Enkele stappen uitvoeren via een lijst herhalen

U kunt gebruikmaken van het [type foreach](app-service-logic-loops-and-scopes.md) om via een matrix van maximaal 10 k items te herhalen en een actie uitvoeren voor elk label.

## <a name="a-failure-handling-step-if-something-goes-wrong"></a>Een stap mislukt foutafhandeling als er iets mis gaat

U vaak wilt kunnen een *remediation stap* schrijven, sommige logica die wordt uitgevoerd, als **en alleen als**, een of meer van uw oproepen is mislukt. In dit voorbeeld zijn we gegevens ophalen uit diverse locaties, maar als het gesprek is mislukt, ik wil ergens een bericht plaatsen zodat ik deze fout later kunt vinden:  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "postToErrorMessageQueue": {
            "type": "ApiConnection",
            "inputs": "...",
            "runAfter": {
                "readData": ["Failed"]
            }
        }
    },
    "outputs": {}
}
```

U kunt aanbrengen gebruik van de `runAfter` eigenschap om op te geven de `postToErrorMessageQueue` moet alleen worden uitgevoerd nadat `readData` is **mislukt**.  Dit kan ook een lijst met mogelijke waarden, dus zijn `runAfter` kan worden `["Succeeded", "Failed"]`.

Ten slotte, omdat u nu de fout verwerkt hebt, we niet langer de uitvoeren als markeren **mislukt**. Zoals u hier zien kunt, is deze klaar **geslaagd** Hoewel één stap mislukt, omdat ik de stap voor het verwerken van deze fout geschreven.

## <a name="two-or-more-steps-that-execute-in-parallel"></a>Twee (of meer) stappen die parallel uitvoeren

Meerdere acties worden uitgevoerd in parallel, hebben de `runAfter` eigenschap moet equivalente gedurende runtime. 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "branch1": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        },
        "branch2": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        }
    },
    "outputs": {}
}
```

Zoals u in het voorbeeld hierboven, beide ziet `branch1` en `branch2` worden ingesteld om uit te voeren na `readData`. Hierdoor worden beide van deze vertakkingen in parallel uitgevoerd:

![Parallel](./media/app-service-logic-author-definitions/parallel.png)

Het tijdstempel voor beide vertakkingen identiek is, kunt u zien. 

## <a name="join-two-parallel-branches"></a>Deelnemen aan twee parallelle vertakkingen

U kunt deelnemen aan twee acties die zijn ingesteld om uit te voeren parallel door items toevoegen aan de `runAfter` eigenschap strekking hierboven.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-04-01-preview/workflowdefinition.json#",
    "actions": {
        "readData": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        },
        "branch1": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "branch2": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "join": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "branch1": [
                    "Succeeded"
                ],
                "branch2": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        }
    },
    "contentVersion": "1.0.0.0",
    "outputs": {},
    "parameters": {},
    "triggers": {
        "manual": {
            "inputs": {
                "schema": {}
            },
            "kind": "Http",
            "type": "Request"
        }
    }
}
```

![Parallel](./media/app-service-logic-author-definitions/join.png)

## <a name="mapping-items-in-a-list-to-some-different-configuration"></a>Items in een lijst toewijzen aan enkele andere configuratie

Volgende, Stel dat we zodat volledig verschillende inhoud afhankelijk van een waarde van een eigenschap wilt maken. We kunt een kaart van waarden naar bestemmingen als een parameter maken:  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "specialCategories": {
            "defaultValue": ["science", "google", "microsoft", "robots", "NSA"],
            "type": "Array"
        },
        "destinationMap": {
            "defaultValue": {
                "science": "http://www.nasa.gov",
                "microsoft": "https://www.microsoft.com/en-us/default.aspx",
                "google": "https://www.google.com",
                "robots": "https://en.wikipedia.org/wiki/Robot",
                "NSA": "https://www.nsa.gov/"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "getArticles": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.wired.com/wired/index"
            },
            "conditions": []
        },
        "getSpecialPage": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
            },
            "conditions": [{
                "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)"
            }],
            "forEach": "@body('getArticles').responseData.feed.entries"
        }
    }
}
```

In dit geval we eerst een lijst met artikelen ophalen en vervolgens de tweede stap opgezocht in een kaart, op basis van de categorie die is gedefinieerd als een parameter, welke URL moet worden zodat de inhoud uit. 

Twee items hier aandacht: de [`intersection()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) functie wordt gebruikt om te controleren of de categorie overeenkomt met een van de bekende categorieën gedefinieerd. Als we de categorie, kunnen we tweede, het item van de map met vierkante haken halen: `parameters[...]`. 

## <a name="working-with-strings"></a>Werken met tekenreeksen

Er zijn diverse functies die kunnen worden gebruikt voor het bewerken van de tekenreeks. U gaat nu een voorbeeld waar we hebben een tekenreeks die we wilt doorgeven aan een systeem, maar we niet er zeker van te zijn dat-codering wordt uitgevoerd correct zijn. Eén optie is naar base64 coderen van deze tekenreeks. Om te voorkomen dat niet zijn toegestaan in een URL gaan we echter een paar tekens wilt vervangen. 

Willen we ook een subtekenreeks van de van de order een naam geven, omdat de eerste 5 tekens niet worden gebruikt.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1",
                "orderer": "NAME=Stèphén__Šīçiłianö"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').orderer,5,sub(length(parameters('order').orderer), 5) )),'+','-') ,'/' ,'_' )}"
            }
        }
    },
    "outputs": {}
}
```

Werken vanaf de binnenstebuiten:

1. Krijgen de [`length()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) van de naam van de besteller dit geeft weer het totaal aantal tekens

2. Aftrekken van 5 (omdat we voorkeur tekenreeks korter)

3. Pas de [`substring()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring) . We beginnen bij index `5` en gaat u de rest van de tekenreeks.

4. Deze reeks die u wilt converteren een [`base64()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) tekenreeks

5. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)alle de `+` met tekens`-`

6. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)alle de `/` met tekens`_`

## <a name="working-with-date-times"></a>Werken met datums en tijden

Datums en tijden kan handig zijn met name wanneer u probeert om gegevens te halen uit een gegevensbron die **Triggers**natuurlijk niet ondersteunen.  U kunt datums en tijden ook gebruiken om te bepalen hoe lang verschillende stappen duurt. 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{parameters('order').id}"
            }
        },
        "timingWarning": {
            "actions" {
                "type": "Http",
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
                },
                "runAfter": {}
            }
            "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))"
        }
    },
    "outputs": {}
}
```

In dit voorbeeld we zijn extraheren van de `startTime` van de vorige stap. We zijn voor het ophalen van de huidige tijd en af te trekken van een tweede:[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) (u kunt andere tijdseenheden, zoals `minutes` of `hours`). Ten slotte, kunnen we deze twee waarden vergelijken. Als de eerste lager is dan de tweede en vervolgens dat betekent meer dan één seconde is verstreken sinds de volgorde eerste is geplaatst. 

Ook de notitie die we tekenreeks formatters gebruiken kunt voor het opmaken van datums: in de queryreeks ik gebruiken [`utcnow('r')`](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow) om de RFC1123. Alle datum opmaak [op MSDN wordt beschreven](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow). 

## <a name="using-deployment-time-parameters-for-different-environments"></a>Met behulp van parameters voor implementatie-tijd voor verschillende omgevingen

Meestal wordt de levenscyclus van een implementatie hebt waarvoor u een ontwikkelomgeving, een testomgeving en klikt u vervolgens een productieomgeving hebben. In alle van de volgende u kunt dezelfde definitie, maar andere database, bijvoorbeeld gebruiken. U wilt ook dezelfde definitie gebruiken op veel verschillende regio's voor beschikbaarheid, maar elk exemplaar van de app logica voor een gesprek aan van de regio-database. 

Houd er rekening mee dat deze verschilt van de verschillende parameters gedurende *runtime*, dat u moet gebruiken voor het duurt het `trigger()` werkt zoals hierboven wordt genoemd. 

U kunt beginnen met een erg eenvoudig definitie zoals dit:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "uri": {
            "type": "string"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('uri')"
            }
        }
    },
    "outputs": {}
}
```

Vervolgens in de werkelijke `PUT` aanvragen voor de logica-app kunt u de parameter bieden `uri`. Let erop, als er niet langer een standaardwaarde deze parameter is vereist in de logica app nettolading:

```
{
    "properties": {},
        "definition": {
          // Use the definition from above here
        },
        "parameters": {
            "connection": {
                "value": "https://my.connection.that.is.per.enviornment"
            }
        }
    },
    "location": "westus"
}
``` 

In elke omgeving kunt u een andere waarde voor Geef de `connection` parameter. 

Zie de [documentatie van de REST API](https://msdn.microsoft.com/library/azure/mt643787.aspx) voor alle van de opties die u hebt voor het maken en logica apps beheren. 
