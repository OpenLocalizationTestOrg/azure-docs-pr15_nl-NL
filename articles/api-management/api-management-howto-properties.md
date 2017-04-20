<properties 
    pageTitle="Het gebruik van eigenschappen in beleidsregels voor informatiebeheer van Azure-API" 
    description="Informatie over het gebruik van eigenschappen in beleidsregels voor informatiebeheer van Azure-API." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


# <a name="how-to-use-properties-in-azure-api-management-policies"></a>Het gebruik van eigenschappen in beleidsregels voor informatiebeheer van Azure-API

Beleidsregels voor informatiebeheer API zijn een krachtige videomogelijkheden van het systeem waarmee de uitgever die u wilt wijzigen van het gedrag van de API tot en met de configuratie. Beleidsregels zijn antwoord van een API of een verzameling instructies die opeenvolging op het verzoek worden uitgevoerd. Beleid-instructies kunnen worden samengesteld met letterlijke tekstwaarden, beleid expressies en eigenschappen. 

Elk exemplaar van de service API Management heeft een eigenschappen verzameling sleutel/waardeparen worden algemene in het exemplaar van de service. Deze eigenschappen kunnen worden gebruikt voor het beheren van constante tekenreekswaarden voor alle API-configuratie en beleid. Elke eigenschap heeft de volgende kenmerken.


| Kenmerk | Type            | Beschrijving                                                                                             |
|-----------|-----------------|---------------------------------------------------------------------------------------------------------|
| Naam      | tekenreeks          | De naam van de eigenschap. Mogelijk alleen letters, cijfers, termijn, streepje bevatten en onderstrepingstekens. |
| Waarde     | tekenreeks          | De waarde van de eigenschap. Deze mogelijk niet leeg zijn of alleen uit witruimte bestaan.                           |
| Geheim    | Booleaanse waarde         | Bepaalt of de waarde geheim en al dan niet moet worden gecodeerd.                                |
| Labels      | matrix van tekenreeks | Optioneel tags die de opgegeven kunnen worden gebruikt om de eigenschappenlijst filteren.                               |

Eigenschappen zijn geconfigureerd in de publisher-portal op het tabblad **Eigenschappen** . In het volgende voorbeeld, zijn drie eigenschappen geconfigureerd.

![Eigenschappen][api-management-properties]

Eigenschapswaarden kunnen letterlijke tekenreeksen en [beleid expressies](https://msdn.microsoft.com/library/azure/dn910913.aspx)bevatten. De volgende tabel ziet u de eigenschappen van het vorige voorbeeld van drie en bijbehorende kenmerken. De waarde van `ExpressionProperty` is een beleidsexpressie die resulteert in een tekenreeks met de huidige datum en tijd. De eigenschap `ContosoHeaderValue` is gemarkeerd als een geheim, zodat de waarde niet wordt weergegeven.

| Naam               | Waarde                      | Geheim | Labels    |
|--------------------|----------------------------|--------|---------|
| ContosoHeader      | TrackingId                 | ONWAAR  | Contoso |
| ContosoHeaderValue | ••••••••••••••••••••••     | Waar   | Contoso |
| ExpressionProperty | @(DateTime.Now.ToString()) | ONWAAR  |         |

## <a name="to-use-a-property"></a>Met behulp van een eigenschap

Als u een eigenschap in een beleid, plaatst u de naam van de eigenschap binnen een paar dubbele accolades zoals `{{ContosoHeader}}`, zoals weergegeven in het volgende voorbeeld.

    <set-header name="{{ContosoHeader}}" exists-action="override">
      <value>{{ContosoHeaderValue}}</value>
    </set-header>

In dit voorbeeld `ContosoHeader` wordt gebruikt als de naam van een koptekst in een `set-header` beleid, en `ContosoHeaderValue` wordt gebruikt als de waarde van deze kop. Als dit beleid wordt geëvalueerd tijdens een aanvraag of antwoord tot de API Management gateway, `{{ContosoHeader}}` en `{{ContosoHeaderValue}}` worden vervangen door de desbetreffende eigenschapswaarden.

Eigenschappen kunnen worden gebruikt als voltooid kenmerk of elementwaarden zoals weergegeven in het vorige voorbeeld, maar ze kunnen ook worden ingevoegd in- of gecombineerd met deel van een expressie letterlijke tekst zoals in het volgende voorbeeld:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Eigenschappen kunnen ook beleid expressies bevatten. Klik in het volgende voorbeeld wordt de `ExpressionProperty` wordt gebruikt.

    <set-header name="CustomHeader" exists-action="override">
        <value>{{ExpressionProperty}}</value>
    </set-header>

Als dit beleid wordt geëvalueerd, `{{ExpressionProperty}}` wordt vervangen door de bijbehorende waarde: `@(DateTime.Now.ToString())`. Aangezien de waarde van een beleidsexpressie is, wordt de expressie wordt geëvalueerd en wordt het beleid verloopt met de uitvoering.

U kunt deze out in de developer portal testen door te bellen van een bewerking met een beleid voor eigenschappen in het bereik. Klik in het volgende voorbeeld wordt een bewerking wordt aangeroepen met het vorige voorbeeld van twee `set-header` beleidsregels met eigenschappen. Houd er rekening mee dat het antwoord bevat twee aangepaste headers die zijn geconfigureerd met beleid met eigenschappen.

![Developer portal][api-management-send-results]

Als u de [API controle doelcellen](api-management-howto-api-inspector.md) voor een oproep waarin de twee vorige voorbeeld beleidsregels met eigenschappen bekijkt, ziet u de twee `set-header` beleidsregels met eigenschapswaarden ingevoegd en de evaluatie van de expressie beleid voor de eigenschap die deel uitmaakt van de beleidsexpressie.

![API controle doelcellen][api-management-api-inspector-trace]

Houd er rekening mee dat terwijl eigenschapswaarden beleid expressies bevatten kunnen, eigenschapswaarden geen andere eigenschappen. Als tekst met een overzicht van de eigenschap wordt gebruikt voor de waarde van een eigenschap, zoals `Property value text {{MyProperty}}`, die eigenschapverwijzing won't vervangen en wordt deel uit van de waarde van de eigenschap.

## <a name="to-create-a-property"></a>Een eigenschap maken

Als u wilt een eigenschap maken, klikt u op de **eigenschap toevoegen** op het tabblad **Eigenschappen** .

![Eigenschap toevoegen][api-management-properties-add-property-menu]

**Naam** en de **waarde** zijn vereiste waarden. Als deze waarde onroerend goed een geheim is, controleert u het selectievakje **Dit is een geheim** . Voer een of meer optionele tags om u te helpen bij het organiseren van uw eigenschappen en klik op **Opslaan**.

![Eigenschap toevoegen][api-management-properties-add-property]

Wanneer een nieuwe eigenschap wordt opgeslagen, wordt het tekstvak **Zoeken eigenschap** wordt gevuld met de naam van de nieuwe eigenschap en wordt de nieuwe eigenschap wordt weergegeven. Als u wilt weergeven op alle eigenschappen, schakelt u het tekstvak **Zoeken eigenschap** en druk op enter.

![Eigenschappen][api-management-properties-property-saved]

Zie voor informatie over het maken van een eigenschap met behulp van de REST API [maken een eigenschap met behulp van de REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).

## <a name="to-edit-a-property"></a>Een eigenschap bewerken

Als u wilt een eigenschap bewerken, klikt u naast de eigenschap bewerken op **bewerken** .

![Eigenschap bewerken][api-management-properties-edit]

Breng de gewenste wijzigingen aan en klik op **Opslaan**. Als u de naam van de eigenschap wijzigt, worden alle beleidsregels die verwijzen naar die eigenschap worden automatisch bijgewerkt als de nieuwe naam wilt gebruiken.

![Eigenschap bewerken][api-management-properties-edit-property]

Zie [een eigenschap met behulp van de REST API bewerken](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch)voor informatie over het bewerken van een eigenschap met behulp van de REST API.

## <a name="to-delete-a-property"></a>Een eigenschap wilt verwijderen

Als u wilt een eigenschap verwijderen, klikt u op **verwijderen** naast de eigenschap te verwijderen.

![Eigenschap verwijderen][api-management-properties-delete]

Klik op **Ja, verwijdert u deze** om te bevestigen.

![Verwijderen bevestigen][api-management-delete-confirm]

>[AZURE.IMPORTANT] Als de eigenschap wordt verwezen door beleid, kunt u zich niet kunt verwijderen totdat u de eigenschap verwijderen uit alle beleidsregels dat deze gebruikt.

Zie [een eigenschap met behulp van de REST API verwijderen](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete)voor informatie over het verwijderen van een eigenschap met behulp van de REST API.

## <a name="to-search-and-filter-properties"></a>Om te zoeken en filteren eigenschappen

Het tabblad **Eigenschappen** bevat zoeken en filteren van mogelijkheden voor het beheren van uw eigenschappen. Als u wilt filteren de lijst met velden op de naam van eigenschap, voert u een zoekterm in het tekstvak **Zoeken eigenschap** . Als u wilt weergeven op alle eigenschappen, schakelt u het tekstvak **Zoeken eigenschap** en druk op enter.

![Zoeken][api-management-properties-search]

Voer een of meer tags in het tekstvak **filteren op tags** wilt filteren de lijst met velden op tag waarden. Als u wilt weergeven op alle eigenschappen, schakel het selectievakje het tekstvak **filteren op labels** en druk op enter.

![Filter][api-management-properties-filter]

## <a name="next-steps"></a>Volgende stappen

-   Meer informatie over het werken met beleid
    -   [Beleid voor het beheer van de API ter](api-management-howto-policies.md)
    -   [Beleidsverwijzing](https://msdn.microsoft.com/library/azure/dn894081.aspx)
    -   [Beleid expressies](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a>Bekijk een video-overzicht

> [AZURE.VIDEO use-properties-in-policies]

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

