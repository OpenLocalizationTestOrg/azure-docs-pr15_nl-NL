<properties
    pageTitle="Toevoegen caching versnellen in Azure API Management | Microsoft Azure"
    description="Meer informatie over het verbeteren van de latentie, bandbreedtegebruik en webservice laden voor API Management service-oproepen."
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
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="add-caching-to-improve-performance-in-azure-api-management"></a>Caching versnellen in Azure API Management toevoegen

Bewerkingen in API Management kunnen worden geconfigureerd voor het opslaan van het antwoord in de cache. Antwoord caching kan aanzienlijk API latentie, bandbreedtegebruik, verkleinen en web service laden voor gegevens die niet vaak worden gewijzigd.

Deze handleiding ziet u hoe u het antwoord in cache opslaan voor uw API toevoegen en configureren van beleidsregels voor de steekproef Echo API-bewerkingen. Vervolgens kunt u de bewerking aanroepen vanuit de developer portal om te controleren of caching in actie.

>[AZURE.NOTE] Voor informatie over de cache items op de toets beleid expressies gebruiken, raadpleegt u [aangepaste caching in Azure API Management](api-management-sample-cache-by-key.md).

## <a name="prerequisites"></a>Vereisten voor

Voordat u de stappen in deze handleiding uitvoert, hebt u een exemplaar van de service API Management met een API en configuratie van een product. Als u een exemplaar van de service API Management nog niet hebt gemaakt, raadpleegt u [een exemplaar van de service API Management maken][] in deze zelfstudie [aan de slag met Azure API Management][] .

## <a name="configure-caching"> </a>Een bewerking voor caching configureren

In deze stap wordt u de instellingen van de cache van de bewerking **Ophalen Resource (in cache)** van de steekproef Echo API controleren.

>[AZURE.NOTE] Elk exemplaar van de service API Management wordt geleverd vooraf geconfigureerde met een Echo-API die kunnen worden gebruikt om te experimenteren met en meer informatie over API Management. Zie [aan de slag met Azure API Management][]voor meer informatie.

Als u wilt beginnen, klikt u op **beheren** in de klassieke Azure-Portal voor uw service API Management. Hiermee gaat u naar de publisher-API-beheerportal.

![Publisher-portal][api-management-management-console]

**API's** in het menu **API beheer** aan de linkerkant op en klik vervolgens op **Echo API**.

![Echo-API][api-management-echo-api]

Klik op het tabblad **bewerkingen** en klik vervolgens op de bewerking **Ophalen Resource (in cache)** in de lijst met **bewerkingen** .

![Echo API-bewerkingen][api-management-echo-api-operations]

Klik op het tabblad **Caching** om weer te geven van de instellingen van de cache voor deze bewerking.

![In de cache tabblad][api-management-caching-tab]

Als u een bewerking in cache opslaan, selecteert u het selectievakje **inschakelen** in. In dit voorbeeld is caching ingeschakeld.

Elk antwoord bewerking is gekoppeld, op basis van de waarden in de velden **variëren op tekenreeks queryparameters** en **variëren op kopteksten** . Als u meerdere antwoorden op basis van de queryreeks-parameters of kopteksten in cache wilt, kunt u ze kunt configureren in deze twee velden.

**Duur** geeft het interval voor het verlopen van de antwoorden in de cache opgeslagen. In dit voorbeeld is het interval **3600** seconden, die gelijk is aan één uur.

Het eerste verzoek om de bewerking **Ophalen Resource (in cache)** retourneert met de in de cache configuratie in dit voorbeeld, een antwoord van de backend-service. Dit antwoord wordt in de cache opgeslagen, maar sleutelveld door de opgegeven kop- en queryreeks-parameters. Mislukken volgende oproepen naar de bewerking met overeenkomende parameters, heeft het in de cache opgeslagen antwoord geretourneerd, totdat de cache duur interval is verlopen.

## <a name="caching-policies"> </a>Het in de cache beleid controleren

In deze stap geeft controleren u de instellingen van de cache voor de bewerking **Ophalen Resource (in cache)** van de steekproef Echo API.

Wanneer de cache-instellingen zijn geconfigureerd voor een bewerking op het tabblad **Caching** , worden in de cache beleidsregels voor de bewerking toegevoegd. Dit beleid kunnen worden bekeken en bewerkt in de editor.

**Beleid** op in het menu **API beheer** aan de linkerkant en selecteer vervolgens **Echo API / ophalen van Resource (in cache)** uit de vervolgkeuzelijst **bewerking** .

![Beleid bereik bewerking][api-management-operation-dropdown]

Hiermee worden de beleidsregels voor deze bewerking weergegeven in de editor.

![API Management beleid-editor][api-management-policy-editor]

De definitie van het beleid voor deze bewerking bevat de beleidsregels die de in de cache configuratie definiëren die zijn gecontroleerd op het tabblad **cache** in de vorige stap.

    <policies>
        <inbound>
            <base />
            <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
                <vary-by-header>Accept</vary-by-header>
                <vary-by-header>Accept-Charset</vary-by-header>
            </cache-lookup>
            <rewrite-uri template="/resource" />
        </inbound>
        <outbound>
            <base />
            <cache-store caching-mode="cache-on" duration="3600" />
        </outbound>
    </policies>

>[AZURE.NOTE] Wijzigingen in de cache beleidsregels in de editor worden doorgevoerd op het tabblad **Caching** van een bewerking en vice versa.

## <a name="test-operation"> </a>Bellen van een bewerking en test de cache opslaan

Als u wilt zien caching in actie, kunnen we de bewerking bellen vanuit de developer portal. Klik op **Developer portal** in het bovenste rechts menu.

![Developer portal][api-management-developer-portal-menu]

**API's** op in het bovenste menu en selecteer vervolgens **Echo API**.

![Echo-API][api-management-apis-echo-api]

>Als u slechts één API geconfigureerde of zichtbaar is voor uw account hebt, gaat klikt op API's u rechtstreeks naar de bewerkingen voor die API.

Selecteer de bewerking **Ophalen Resource (in cache)** en klik op **Openen Console**.

![Geopende console][api-management-open-console]

De beheerconsole kunt u bewerkingen rechtstreeks vanuit de developer portal aanroepen.

![Console][api-management-console]

Houd de standaardwaarden voor **param1** en **param2**.

Selecteer de gewenste sleutel in de vervolgkeuzelijst **abonnement-toets** . Als uw account slechts één abonnement heeft, wordt dit is geselecteerd.

Voer **sampleheader:value1** in het tekstvak voor **kopteksten aanvragen** .

Klik op **HTTP Get** en noteer de koppen antwoord.

**Sampleheader:value2** invoeren in het tekstvak voor **kopteksten aanvragen** en klik vervolgens op **HTTP Get**.

Houd er rekening mee dat de waarde van **sampleheader** nog steeds **waarde1** in de reactie. Probeert u enkele andere waarden en houd er rekening mee dat het in de cache opgeslagen antwoord van de eerste oproep wordt geretourneerd.

Typ **25** in het veld **param2** en klik vervolgens op **HTTP Get**.

Houd er rekening mee dat de waarde van **sampleheader** in het antwoord nu **waarde2 is**. Omdat de Bewerkingsresultaten zijn sleutelvelden door queryreeks, is niet het vorige in de cache opgeslagen antwoord geretourneerd.

## <a name="next-steps"> </a>Vervolgstappen

-   Zie [Caching van beleid][] in de [API Management beleidsverwijzing][]voor meer informatie over het in de cache.
-   Voor informatie over de cache items op de toets beleid expressies gebruiken, raadpleegt u [aangepaste caching in Azure API Management](api-management-sample-cache-by-key.md).

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Aan de slag met Azure API Management]: api-management-get-started.md

[API Management beleidsverwijzing]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[In de cache beleidsregels]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Een exemplaar van de service API Management maken]: api-management-get-started.md#create-service-instance

[Configure an operation for caching]: #configure-caching
[Review the caching policies]: #caching-policies
[Call an operation and test the caching]: #test-operation
[Next steps]: #next-steps
