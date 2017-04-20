<properties 
    pageTitle="Azure API Management beleidsverwijzing" 
    description="Meer informatie over het beleid dat beschikbaar is voor het configureren van API-Management." 
    services="api-management" 
    documentationCenter="" 
    authors="vladvino" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="apimpm"/>

# <a name="azure-api-management-policy-reference"></a>Azure API Management beleidsverwijzing

Deze sectie bevat een index voor het beleid in de [API Management beleidsverwijzing][]. Zie voor informatie over het toevoegen en configureren van beleidsregels, [beleid voor het beheer van de API ter][].

Beleid expressies kunnen worden gebruikt als kenmerkwaarden of tekst in een van de API beleidsregels voor, tenzij anderszins Hiermee geeft u het beleid. Bepaalde beleidsinstellingen zoals het beleid [besturingsstroom][] en [stelt u variabele][] zijn gebaseerd op beleid expressies. Zie voor meer informatie, [Geavanceerde beleidsregels][] en [beleid expressies][]

## <a name="policy-reference-index"></a>Beleid verwijzing index

-   [Beperking clienttoegangsbeleid][]
    -   [Schakel HTTP-header][] - wordt afgedwongen bestaan en/of de waarde van een HTTP-koptekst.
    -   [Limiet gesprek tarieven per abonnement][] - voorkomt dat API-gebruik bereikt door te bellen rente, op basis van de per abonnement beperken.
    -   [Limiet gesprek tarief door sleutel](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) - voorkomt dat API-gebruik bereikt door te bellen rente, op basis van de sleutel per beperken.
    -   [Beperken beller IP-adressen][] - Filters (kunt/al dan niet toestaan) oproepen van specifieke IP-adressen en/of -adresbereiken.
    -   [Quota Resourcegebruik door abonnement instellen][] -, kunt u een vernieuwd of levensduur gesprek volume en/of de bandbreedte quotum, op basis van de per abonnement afdwingen.
    -   [Quota Resourcegebruik door sleutel instellen](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) -, kunt u een vernieuwd of levensduur gesprek volume en/of de bandbreedte quotum, op basis van de sleutel per afdwingen.
    -   [Valideer de JWT][] - bestaan en de geldigheid van een JWT opgehaald uit een opgegeven HTTP-koptekst of een opgegeven queryparameter afdwingt.
-   [Geavanceerde beleidsregels][]
    -   [Besturing][] - geldt voorwaardelijk beleid overzichten op basis van de resultaten van de evaluatie van Booleaanse [expressies][].
    -   [Aanvragen doorsturen][] - de aanvraag doorgestuurd naar de backend-service.
    -   [Log op gebeurtenis Hub][] - verzonden berichten in de opgegeven notatie met een bericht doel gedefinieerd door een [logboek](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) entiteit.
    -   [Probeer](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) - pogingen uitvoering van de ingesloten beleid-instructies, als en totdat de voorwaarde is voldaan. Uitvoering wordt met de opgegeven tussenpozen te herhalen en probeer tot aan het opgegeven aantal.
    -   [Antwoord retourneren](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) - wordt afgebroken verkooppijplijn worden uitgevoerd en geeft als resultaat het opgegeven antwoord rechtstreeks naar de beller.
    -   [Verzend een manier verzoek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) - verzendt een verzoek naar de opgegeven URL zonder te wachten op een reactie.
    -   [Verzend verzoek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) -, wordt een aanvraag verzonden naar de opgegeven URL.
    -   [Set verzoek methode](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) - kunt u de HTTP-methode voor een verzoek om te wijzigen.
    -   [Status instellen](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) - de HTTP-statuscode wordt gewijzigd in de opgegeven waarde.
    -   [Stel variabele][] - aanhouden van een waarde in een variabele benoemde [context][] voor later toegang.
    -   [Doelcellen](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) - voegt u een tekenreeks in de uitvoer [API controle](../api-management/api-management-howto-api-inspector.md) .
    -   [Wacht](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) - wacht ingesloten verzoek om te verzenden, Get-waarde uit de cache of het besturingselement stroom beleid om te voltooien voordat u verdergaat.
-   [Beleidsregels voor verificatie][]
    -   [Verifiëren met Basic][] - verifiëren met een backend-service met behulp van basisverificatie.
    -   [Verifiëren met clientcertificaat][] - verifiëren met een backend-service met clientcertificaten.
-   [In de cache beleidsregels][] 
    -   [Ophalen uit de cache][] - cache uitvoeren opzoeken en een geldig in de cache opgeslagen antwoord indien beschikbaar terug te keren.
    -   [Store aan cache][] - cache antwoord op basis van de opgegeven cache configuratie.
    -   [Haal de waarde uit de cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) - ophalen een item in de cache opgeslagen door sleutel.
    -   [Store-waarde in de cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) - Store een item in de cache op drukt.
    -   [Verwijder de waarde uit de cache](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) - verwijderen een item in de cache op drukt.
-   [Cross beleid voor het domein][] 
    -   [Oproepen van de andere domeinen toestaan][] - maakt de API toegankelijk vanuit Adobe Flash en Microsoft Silverlight browser-clients.
    -   [CORS][] - worden opgeteld cross-origin resource delen (CORS)-ondersteuning naar een bewerking of een API toe te staan dat oproepen tussen domeinen vanaf browser-clients.
    -   [JSONP][] - JSON worden toegevoegd met ondersteuning voor een bewerking opvulling (JSONP) of een API toe te staan dat oproepen tussen domeinen vanaf JavaScript browser-clients.
-   [Beleidsregels voor transformatie][] 
    -   [JSON converteren naar XML][] - worden omgezet verzoek om een antwoord of platte van JSON naar XML.
    -   [XML-converteren naar JSON][] - worden omgezet verzoek om een antwoord of platte van XML-naar JSON.
    -   [Zoeken en vervangen van de tekenreeks in de hoofdtekst][] - vindt u een verzoek om een antwoord of subtekenreeks en vervangen door een andere subtekenreeks.
    -   [URL's masker in inhoud][] - koppelingen opnieuw schrijft (maskers) in het antwoord platte zodat ze verwijzen naar de overeenkomstige koppeling via de gateway.
    -   De backend-service voor een binnenkomende aanvraag [instellen backend-service][] - worden gewijzigd.
    -   [Hoofdtekst instellen][] - Hiermee stelt u de berichttekst voor binnenkomende en uitgaande aanvragen.
    -   [Koptekst instellen HTTP][] - waarde wordt toegewezen aan een bestaande antwoord en/of de kop van de aanvraag of een nieuwe antwoord en/of verzoek koptekst toevoegt.
    -   [Instellen van de queryreeksparameter][] - toevoegt, vervangt waarde van of verwijdert queryreeksparameter aanvragen.
    -   [URL herschrijven][] - converteert een aanvraag-URL van de openbare vorm aan het formulier door de webservice verwacht.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over expressies beleid, de volgende video.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Beperking clienttoegangsbeleid]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[HTTP-header controleren]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[Limiet gesprek tarieven per abonnement]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[Beller IP-adressen beperken]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[De gebruiksquota instellen door abonnement]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[JWT valideren]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[Geavanceerde beleidsregels]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Besturing]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variabele]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[expressies]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[context]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables
[Aanvragen doorsturen]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest
[Log op gebeurtenis Hub]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Beleidsregels voor verificatie]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[Verificatie met Basic]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Verificatie met clientcertificaat]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[In de cache beleidsregels]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Ophalen uit de cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[In cache opslaan]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Cross beleid voor het domein]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Andere domeinen oproepen toestaan]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Beleidsregels voor transformatie]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[JSON converteren naar XML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[XML converteren naar JSON]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[Zoeken en vervangen van de tekenreeks in de hoofdtekst]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[URL's masker in inhoud]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[Set backend-service]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[Hoofdtekst instellen]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[HTTP-koptekst instellen]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[Parameter queryreeks instellen]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[URL herschrijven]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[Beleid voor het beheer van de API ter]: api-management-howto-policies.md
[API Management beleidsverwijzing]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[Beleid expressies]: https://msdn.microsoft.com/library/azure/dn910913.aspx

 
