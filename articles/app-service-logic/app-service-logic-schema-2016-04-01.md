<properties 
    pageTitle="Nieuwe schemaversie 2016-01-06 | Microsoft Azure" 
    description="Meer informatie over het schrijven van de JSON-definitie voor de meest recente versie van de logica apps" 
    authors="jeffhollan" 
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
    ms.date="07/25/2016"
    ms.author="jehollan"/>
    
# <a name="new-schema-version-2016-06-01"></a>Nieuwe schemaversie 2016-01-06

Het nieuwe schema en API versie voor logica apps heeft een aantal verbeteringen die de betrouwbaarheid verbeteren en eenvoudig te gebruiken met logica apps. Er zijn 3 belangrijke verschillen:

1. Toevoeging met bereiken welke acties die een verzameling acties bevatten.
1. Voorwaarden en lussen zijn eersteklas acties
1. Uitvoering ordening uitgebreidere via `runAfter` eigenschap (welke vervangt `dependsOn`)

Voor meer informatie over het upgraden van uw apps logica van het schema 2015-08-01-preview naar het schema 2016-06-01 [raadpleegt u de upgrade sectie hieronder.](#upgrading-to-2016-06-01-schema)


## <a name="1-scopes"></a>1. bereiken

Een van de grootste wijzigingen in dit schema is de toevoeging van bereiken en de mogelijkheid om te nesten acties binnen elkaar.  Dit is handig wanneer een reeks acties te groeperen of wanneer hoeft te nesten acties binnen elkaar (bijvoorbeeld een voorwaarde kan bevatten een andere voorwaarde).  Meer informatie over de syntaxis van de reikwijdte kunnen worden gevonden [hier](app-service-logic-loops-and-scopes.md), maar het voorbeeld van een eenvoudige bereik vindt u hieronder:


```
{
    "actions": {
        "My_Scope": {
            "type": "scope",
            "actions": {                
                "Http": {
                    "inputs": {
                        "method": "GET",
                        "uri": "http://www.bing.com"
                    },
                    "runAfter": {},
                    "type": "Http"
                }
            }
        }
    }
}
```

## <a name="2-conditions-and-loops-changes"></a>2. voorwaarden en lussen van wijzigingen

In de vorige versies van het schema zijn voorwaarden en lussen parameters die zijn gekoppeld aan een enkele actie.  Deze beperking is opgeheven in dit schema en nu voorwaarden en lussen wordt weergegeven als een type actie.  Meer informatie vindt u [in dit artikel](app-service-logic-loops-and-scopes.md)en een eenvoudig voorbeeld van de actie van een voorwaarde is hieronder:

```
{
    "If_trigger_is_foo": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'foo')",
        "runAfter": { },
        "actions": {
            "Http_2": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.bing.com"
                },
                "runAfter": {},
                "type": "Http"
            }
        },
        "else": 
        {
            "if_trigger_is_bar": "..."
        }      
    }
}
```

## <a name="3-runafter-property"></a>3. de eigenschap RunAfter

De nieuwe `runAfter` eigenschap vervangt `dependsOn` om te helpen exacter toestaan in uitvoeren volgorde.  `dependsOn`is synoniem met 'de bewerking hebt uitgevoerd en is geslaagd,' echter vaak die u moet een actie uitvoeren als de vorige actie geslaagd is, is mislukt of overgeslagen.  `runAfter`kunnen voor deze flexibiliteit.  Er is een object dat Hiermee geeft u alle actienamen die wordt uitgevoerd nadat u hebt en een matrix van de status zijn toegestaan aan trigger uit definieert.  Voor voorbeeld als u wilt uitvoeren na stap A is geslaagd en B is is geslaagd of mislukt, kunt u het volgende wilt samenstellen `runAfter` eigenschap:

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrading-to-2016-06-01-schema"></a>Een upgrade naar 2016-06-01-schema

Een upgrade naar het nieuwe 2016-06-01-schema duurt slechts een paar stappen.  [In dit artikel](app-service-logic-schema-2016-04-01.md)vindt u meer informatie over de wijzigingen van het schema.  Het upgradeproces bevat de upgrade script, op te slaan als een nieuwe logica-app en waarbij oude logica app desgewenst uitvoeren.

1. Open uw huidige logica-app.
1. Klik op de knop **Update Schema** op de werkbalk
   
    ![][1]
   
    De bijgewerkte definitie worden geretourneerd.  U kunt kopiëren en plak deze in de definitie van een resource als u nodig hebt, maar we **raden** u de knop **OpslaanAls** gebruiken om te zorgen dat alle verbinding verwijzingen zijn geldig in de bijgewerkte logica-app.
1. Klik op de knop **OpslaanAls** op de werkbalk van het blad voor de upgrade.
1. Vul de status van de app naam en logica en klik op **maken** om uw app upgrade logica implementeren.
1. Controleer of dat uw app bijgewerkte logica werkt zoals verwacht.

    >[AZURE.NOTE] Als u een handleiding of verzoek trigger gebruikt, wordt de URL voor terugbellen in de nieuwe logica-app zijn gewijzigd.  Gebruik de nieuwe URL voor de verificatie van het end-to-end werkt en u kunt klonen via uw bestaande logica-app te bewaren vorige URL's.

1. *Optionele* Gebruik de knop **klonen** op de werkbalk (naast het pictogram **Update Schema** in de bovenstaande afbeelding) naar de vorige logica app overschrijven met de nieuwe schemaversie.  Dit is alleen nodig als u wilt behouden dezelfde resource-ID of trigger-URL van uw app logica aanvragen.

### <a name="upgrade-tool-notes"></a>Hulpmiddel notities upgraden

#### <a name="condition-mapping"></a>Voorwaarde toewijzing

Het hulpmiddel, kunt u een aanbevolen inspanning om te groeperen van de acties true en false tak samen in een bereik in de bijgewerkte definitie.  Specifiek de ontwerpfunctie patroon van `@equals(actions('a').status, 'Skipped')` moet worden weergegeven als een `else` actie.  Als het hulpmiddel worden gedetecteerd patronen deze niet wordt herkend, wordt mogelijk echter afzonderlijk voorwaarden voor zowel de true als de waarde false tak maken.  Acties kunnen worden opnieuw toegewezen posten upgrade indien nodig.

#### <a name="foreach-with-condition"></a>ForEach met voorwaarde
  
Het vorige patroon van een lus foreach met een voorwaarde per item kan worden gerepliceerd in het nieuwe schema met de filteractie.  Dit gebeurt automatisch op upgrade.  De voorwaarde wordt de filteractie van een voor de loop van de foreach (om terug te keren alleen een matrix van items die overeenkomen met de voorwaarde) en deze matrix wordt doorgegeven aan de actie foreach.  U kunt een voorbeeld van deze [in dit artikel](app-service-logic-loops-and-scopes.md) weergeven

#### <a name="resource-tags"></a>Resource-labels

Resource-codes bij een upgrade worden verwijderd en moet u deze opnieuw instellen voor de bijgewerkte werkstroom.

## <a name="other-changes"></a>Andere wijzigingen

### <a name="manual-trigger-renamed-to-request-trigger"></a>Handmatige trigger gewijzigd in verzoek-trigger

Het type `manual` is afgeschaft en de naam gewijzigd in `request` met het type van `http`.  Dit is nu consistenter met het type patroon die de trigger wordt gebruikt om u te maken.

### <a name="new-filter-action"></a>Nieuwe 'filter' actie

Als u met een groot aantal en moeten deze naar beneden af op een kleinere set items filteren werkt, kunt u het nieuwe 'filter' type.  Deze wordt accepteert een matrix en een voorwaarde en de voorwaarde voor elk item evalueren en retourneren van een matrix van items die aan de voorwaarde voldoen.

### <a name="foreach-and-until-action-restrictions"></a>ForEach en tot actie beperkingen

De foreach en tot lus zijn beperkt tot één enkele actie.

### <a name="trackedproperties-on-actions"></a>TrackedProperties op acties

Acties kunnen nu nog een eigenschap hebben (op hetzelfde niveau naar `runAfter` en `type`) genoemd `trackedProperties`.  Een object dat aangeeft bepaalde actie invoer of uitvoer moet worden opgenomen in de diagnostische Azure telemetrielogboek die wordt gegenereerd als onderdeel van een werkstroom is.  Bijvoorbeeld:

```
{                
    "Http": {
        "inputs": {
            "method": "GET",
            "uri": "http://www.bing.com"
        },
        "runAfter": {},
        "type": "Http",
        "trackedProperties": {
            "responseCode": "@action().outputs.statusCode",
            "uri": "@action().inputs.uri"
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen
- [Gebruik de werkstroomdefinitie logica-app](app-service-logic-author-definitions.md)
- [Een logica app implementatie-sjabloon maken](app-service-logic-create-deploy-template.md)


<!-- Image references -->
[1]: ./media/app-service-logic-schema-2016-04-01/upgradeButton.png
