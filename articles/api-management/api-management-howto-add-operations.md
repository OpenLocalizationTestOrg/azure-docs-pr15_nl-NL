<properties 
    pageTitle="Bewerkingen toevoegen aan een API in Azure API Management | Microsoft Azure" 
    description="Leer hoe u bewerkingen toevoegen aan een API in Azure API Management." 
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

# <a name="how-to-add-operations-to-an-api-in-azure-api-management"></a>Bewerkingen toevoegen aan een API in Azure API Management

Voordat u een API in API Management kan worden gebruikt, moeten u bewerkingen toevoegen. Deze handleiding wordt aangegeven hoe toevoegen en configureren van verschillende typen bewerkingen voor een API voor het beheer van de API ter.

## <a name="add-operation"> </a>Een bewerking toevoegen

Bewerkingen zijn toegevoegd en geconfigureerd voor een API in de publisher-portal. Voor toegang tot de publisher-portal op **beheren** in de klassieke Azure-Portal voor uw service API Management.

![Publisher-portal][api-management-management-console]

>Als u een exemplaar van de service API Management nog niet hebt gemaakt, raadpleegt u [een exemplaar van de service API Management maken][] in deze zelfstudie [aan de slag met Azure API Management][] .

Selecteer de gewenste API in de publisher-portal en selecteer vervolgens het tabblad **bewerkingen** . 

![Bewerkingen][api-management-operations]

Klik op **Bewerking toevoegen** als een nieuwe bewerking wilt toevoegen. De **nieuwe bewerking** wordt weergegeven en wordt het tabblad **handtekening** al dan niet standaard geselecteerd.

![Bewerking toevoegen][api-management-add-operation]

Geef de **HTTP-woord** te kiezen uit de vervolgkeuzelijst.

![HTTP-methode][api-management-http-method]

<a name="url-template"></a>

De URL-sjabloon door te typen in een URL-fragment dat bestaat uit een of meer URL-padsegmenten en nul of meer queryreeks-parameters definiëren. De URL-sjabloon, toegevoegd aan de basis-URL van de API, kunt u een enkele HTTP-bewerking identificeren. Bevat een of meer benoemde variabele onderdelen die door accolades worden geïdentificeerd. Volgende variabele onderdelen Sjabloonparameters worden genoemd en worden waarden opgehaald uit de URL van de aanvraag wanneer de aanvraag wordt verwerkt door het platform-API Management dynamisch toegewezen.

![URL-sjabloon][api-management-url-template]

<a name="rewrite-url-template"></a>

Desgewenst kunt u de **URL herschrijven sjabloon**opgeven. Hiermee kunt u de standaardsjabloon URL gebruiken voor het verwerken van verzoeken voor oproepen op de front-end, tijdens het aanroepen van de back-enddatabase via de URL van een geconverteerde op basis van de sjabloon voor revisie. Sjabloonparameters van de sjabloon URL moeten worden gebruikt in de sjabloon voor revisie. Het volgende voorbeeld ziet u hoe inhoudstype codering als padsegment in de webservice uit het vorige voorbeeld kan worden verstrekt als queryparameter in de API die zijn gepubliceerd via het beheer van de API-platform met de URL-sjablonen.

![URL-sjabloon voor revisie][api-management-url-template-rewrite]

Bellers aan de bewerking gebruikmaken van de indeling `/customers?customerid=ALFKI` en dit wordt toegewezen aan `/Customers('ALFKI')` wanneer de back-enddatabase-service wordt aangeroepen.


**Weergavenaam** en **Beschrijving** een beschrijving van de bewerking en worden gebruikt om te leveren documentatie voor het gebruik van deze API in de developer portal ontwikkelaars.

![Beschrijving][api-management-description]

De beschrijving van de bewerking kan worden opgegeven als tekst zonder opmaak of HTML-code in het vak **Beschrijving** .

## <a name="operation-caching"> </a>Caching van bewerking

Hiermee reduceert u antwoord caching latentie waargenomen door de API consumenten, verlaagt bandbreedtegebruik en -verlagingen de belasting op het web HTTP-service de API implementeren. 

Als u wilt snel en eenvoudig inschakelen voor de bewerking caching, selecteert u het tabblad **Caching** en schakel het selectievakje **inschakelen** in.

![In cache opslaan][api-management-caching-tab]

**Duur** geeft de periode waarin het antwoord bewerking blijft aanwezig in de cache. De standaardwaarde is 3600 seconden of 1 uur staan.

Cache sleutels worden gebruikt om onderscheid te maken tussen antwoorden zodat het antwoord dat overeenkomt met de toets van elke andere cache een eigen afzonderlijk in de cache opgeslagen waarde krijgt. Voer desgewenst specifieke queryreeks-parameters en/of HTTP-headers in de cache-sleutelwaarden in de tekstvakken **variëren op tekenreeks queryparameters** en **variëren op kopteksten** respectievelijk computing moet worden gebruikt. Wanneer geen zijn opgegeven, volledige aanvraag-URL en de volgende HTTP koptekst waarden worden gebruikt in de cache belangrijke generatie: **accepteren** en **Accepteren-tekenset**.

>Zie voor meer informatie over caching en caching van beleidsregels, [het resultaat van de cache voor bewerking in Azure API Management][].


## <a name="request-parameters"> </a>Parameters aanvragen

Bewerkingsparameters worden beheerd op het tabblad Parameters. Parameters zijn opgegeven in de **URL van sjabloon** op het tabblad **handtekening** automatisch worden toegevoegd en kunnen worden gewijzigd alleen door de URL-sjabloon bewerken. Aanvullende parameters, kunnen handmatig worden ingevoerd.

Als u wilt een nieuwe queryparameter hebt toegevoegd, klikt u op **Queryparameter toevoegen** en voer de volgende gegevens:

-   **Naam** - parameternaam.
-   **Beschrijving** - een korte beschrijving van de parameter (optioneel).
-   **Type** - parametertype, geselecteerd in de vervolgkeuzelijst omlaag.
-   **Waarden** - waarden die kunnen worden toegewezen aan deze parameter. Een van de waarden kan worden gemarkeerd als standaard (optioneel).
-   **Vereist** - de parameter verplicht maken door te schakelen van het selectievakje. 

![Parameters aanvragen][api-management-request-parameters]

## <a name="request-body"> </a>Hoofdtekst aanvragen

Als de bewerking toestaat (bijvoorbeeld opslag, bericht) en een hoofdtekst kunt u desgewenst een voorbeeld van het in alle notaties van de ondersteunde weergave (bijvoorbeeld json, XML) vereist. 

>Het hoofdgedeelte van de aanvraag alleen voor documentatie wordt gebruikt en niet worden gevalideerd.

Als u de hoofdtekst van een aanvraag, overschakelen naar het tabblad **hoofdtekst** .

Klik op **Weergave toevoegen**, typ gewenste inhoudstypenaam (bijvoorbeeld toepassing/json), selecteert u deze in de vervolgkeuzelijst en plak het gewenste verzoek hoofdtekst voorbeeld in de geselecteerde indeling in het tekstvak. 

![Hoofdtekst aanvragen][api-management-request-body]

In aanvulling op de vorm, kunt u ook opgeven een optionele beschrijving in het vak **Beschrijving** .

## <a name="responses"> </a>Antwoorden

Dit is een goede gewoonte om vindt u voorbeelden van antwoorden voor alle statuscodes die u kunt de bewerking kan produceren. Elke statuscode wellicht meer dan één antwoord hoofdtekst voorbeeld, één voor elk van de ondersteunde inhoudstypen. 

Als u wilt een antwoord hebt toegevoegd, klikt u op **toevoegen** en typ de gewenste statuscode. In dit voorbeeld de statuscode is **200 OK**. Zodra de code wordt weergegeven in de vervolgkeuzelijst, selecteert u deze en de antwoordcode is gemaakt en toegevoegd aan uw bewerking.

![Antwoordcode][api-management-response-code]

Klik op **Weergave toevoegen**, typ de gewenste inhoudstype-naam (bijvoorbeeld toepassing/json) en selecteer deze in de vervolgkeuzelijst omlaag.

![Inhoudstype van de hoofdtekst][api-management-response-body-content-type]

Plak in het voorbeeld van de hoofdtekst antwoord in de geselecteerde indeling in het tekstvak. 

![Antwoord hoofdtekst][api-management-response-body]

Desgewenst toevoegen een optionele beschrijving in het vak **Beschrijving** .

Zodra de bewerking is geconfigureerd, klikt u op **Opslaan**.


## <a name="next-steps"> </a>Vervolgstappen

Nadat de bewerkingen zijn toegevoegd aan een API, wordt de volgende stap worden naar de API koppelen aan een product en te publiceren zodat ontwikkelaars bewerkingen kunnen bellen.

-   [Het maken en publiceren van een product][]

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Aan de slag met Azure API Management]: api-management-get-started.md
[Een exemplaar van de service API Management maken]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[Het maken en publiceren van een product]: api-management-howto-add-products.md
[Het resultaat van de cache voor bewerking in Azure API Management]: api-management-howto-cache.md