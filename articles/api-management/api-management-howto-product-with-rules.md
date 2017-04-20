<properties
    pageTitle="Beveiligen van uw API met Azure API Management | Microsoft Azure"
    description="Informatie over het beveiligen van uw API met quota en beperken beleid (rente beperking)."
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

# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a>Beveiligen van uw API met limieten voor de snelheid Azure API te beheren

Deze handleiding ziet u hoe gemakkelijk het is om de beveiliging voor uw back-end API toevoegen door te tarief quota en limiet beleidsregels configureren met Azure API Management.

In deze zelfstudie maakt u een "gratis proefversie API' product waarmee ontwikkelaars kunnen bellen maximaal 10 per minuut en tot maximaal 200 oproepen per week aan tot de API gebruik van de [limiet gesprek tarieven per abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) en het beleid voor het [instellen van gebruiksquota per abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) . U vervolgens de API publiceren en testen van het beleid rente.

Zie voor meer geavanceerde bandbreedteregeling scenario's met het [rente-limiet-door-sleutel](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) en [quotum door sleutel](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) beleid, [Geavanceerde aanvraag beperken met Azure API Management](api-management-sample-flexible-throttling.md).

## <a name="create-product"> </a>Maken van een product

In deze stap maakt u een gratis proefversie-product dat er geen abonnement goedkeuring vereist.

>[AZURE.NOTE] Als u al een product dat is geconfigureerd en wilt deze gebruiken voor deze zelfstudie, kunt u gehandhaafd [configureren][] bellen tarief quota en limiet beleidsregels en volgt u de zelfstudie daarvandaan met uw product in plaats van het product gratis proefversie.

Als u wilt beginnen, klikt u op **beheren** in de klassieke Azure voor uw service API Management. Hiermee gaat u naar de publisher-API-beheerportal.

![Publisher-portal][api-management-management-console]

>Als u een exemplaar van de service API Management nog niet hebt gemaakt, raadpleegt u [een exemplaar van de service API Management maken][] in deze zelfstudie [beheren uw eerste API in Azure API Management][] .

Klik op **producten** in het menu **API beheer** aan de linkerkant om weer te geven van de pagina **producten** .

![Product toevoegen][api-management-add-product]

Klik op **product toevoegen** als u wilt weergeven in het dialoogvenster **Nieuw product toevoegen** .

![Nieuw product toevoegen][api-management-new-product-window]

Typ in het vak **titel** **Gratis proefversie**.

Typ in het vak **Beschrijving** de volgende tekst:  **abonnees kunnen uitvoeren 10 oproepen/minuten tot maximaal 200 oproepen/week, waarna toegang aan.**

Producten in API Management kunnen worden beveiligd of open. Beveiligde producten moeten zijn geabonneerd op voordat ze kunnen worden gebruikt. Open producten kunnen worden gebruikt zonder een abonnement. Zorg ervoor dat het **vereisen van abonnement** is geselecteerd om te maken van een beveiligde product waarvoor een abonnement. Dit is de standaardinstelling.

Als u een beheerder om te controleren en accepteren of negeren abonnement pogingen voor dit product, schakelt u het **abonnement goedkeuring vereisen**. Als het selectievakje niet is geselecteerd, zijn abonnement pogingen automatisch goedgekeurd. In dit voorbeeld worden abonnementen automatisch goedgekeurd, zodat u niet het vak Selecteer.

Selecteer het selectievakje **toestaan meerdere tegelijk abonnementen** zodat ontwikkelaars accounts zich meerdere keren op het nieuwe product abonneren. Deze zelfstudie niet gebruikmaken van meerdere tegelijk abonnementen, dus laat deze uitgeschakeld.

Nadat u alle waarden worden ingevoerd, klikt u op **Opslaan** om het product te maken.

![Product toegevoegd][api-management-product-added]

Nieuwe producten zijn standaard zichtbaar voor gebruikers in de groep **Administrators** . We gaan om toe te voegen van de groep **ontwikkelaars** . **Gratis proefversie**op en klik vervolgens op het tabblad **zichtbaarheid** .

>Beheer van API worden groepen gebruikt voor het beheren van de zichtbaarheid van producten voor ontwikkelaars. Producten zichtbaarheid verlenen aan groepen en ontwikkelaars kunnen bekijken en abonneren op de producten die zichtbaar zijn voor de groepen waarin ze behoren. Lees [hoe u maken en gebruiken van groepen in Azure API Management][]voor meer informatie.

![Groep ontwikkelaars toevoegen][api-management-add-developers-group]

Schakel het selectievakje **ontwikkelaars** in en klik vervolgens op **Opslaan**.

## <a name="add-api"> </a>Een API toevoegen aan het product

In deze stap van de zelfstudie wordt we de API Echo toevoegen aan het nieuwe gratis proefversie-product.

>Elk exemplaar van de service API Management wordt geleverd vooraf geconfigureerde met een Echo-API die kunnen worden gebruikt om te experimenteren met en meer informatie over API-Management. Zie [uw eerste API in Azure API Management beheren][]voor meer informatie.

**Producten** in het menu **API beheer** aan de linkerkant op en klik vervolgens op **Gratis proefversie** om te configureren van het product.

![Product configureren][api-management-configure-product]

Klik op **Toevoegen API product**.

![API product toevoegen][api-management-add-api]

Selecteer **Echo API**en klik vervolgens op **Opslaan**.

![Echo API toevoegen][api-management-add-echo-api]

## <a name="policies"> </a>Gesprek tarief quota en limiet beleid configureren

Limieten voor de snelheid en quota's zijn in de editor geconfigureerd. Klik op **beleidsregels** onder het menu **API beheer** aan de linkerkant. Klik in de lijst met **producten** , op **Gratis proefversie**.

![Productbeleid][api-management-product-policy]

Klik op **Beleid toevoegen** voor het importeren van de sjabloon voor beleid en begint met het maken van het tarief weer dat quota en limiet beleid.

![Beleid toevoegen][api-management-add-policy]

Als u wilt invoegen beleidsregels, plaats de cursor in de **binnenkomende** of het **uitgaande** gedeelte van de sjabloon voor beleid. Tarief quota en limiet beleidsregels inkomende beleidsregels, dus plaats de cursor in het inkomende element.

![Beleidseditor][api-management-policy-editor-inbound]

De twee beleidsregels die we in deze zelfstudie toevoegen wilt zijn het beleid [limiet gesprek tarieven per abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) en [gebruiksquota per abonnement instellen](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) .

![Beleid-instructies][api-management-limit-policies]

Nadat de cursor in het **binnenkomende** beleid-element, klikt u op de pijl naast **limiet gesprek tarieven per abonnement** om in te voegen van de sjabloon voor beleid.

    <rate-limit calls="number" renewal-period="seconds">
    <api name="name" calls="number">
    <operation name="name" calls="number" />
    </api>
    </rate-limit>

**Limiet gesprek tarieven per abonnement** kan worden gebruikt op het productniveau van het en kunnen ook worden gebruikt op de API en afzonderlijke bewerking naam niveau. In deze zelfstudie alleen product niveau beleid wordt gebruikt, zodat de **api** en **bewerking** elementen verwijderen uit de **limiet van-** element, zodat alleen de buitenste **limiet** element, blijft, zoals wordt weergegeven in het volgende voorbeeld.

    <rate-limit calls="number" renewal-period="seconds">
    </rate-limit>

In het product gratis proefversie, de maximale toegestane gesprek rente gelijk is aan 10 oproepen per minuut, dus **10** typt als de waarde voor het kenmerk **oproepen** en **60** voor de **verlenging-punt** -kenmerk.

    <rate-limit calls="10" renewal-period="60">
    </rate-limit>

De **gebruiksquota per abonnement instellen** als beleid wilt configureren, plaats de cursor direct onder de toegevoegde **limiet** element binnen het **inkomende** element en klik vervolgens op de pijl links van **de quota Resourcegebruik per abonnement instellen**.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    <api name="name" calls="number" bandwidth="kilobytes">
    <operation name="name" calls="number" bandwidth="kilobytes" />
    </api>
    </quota>

Omdat dit beleid is ook bedoeld om te worden op het productniveau van het, verwijder de **api** en **bewerking** Naamelementen, zoals wordt weergegeven in het volgende voorbeeld.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    </quota>

Quota's worden gebaseerd op het aantal aanroepen per interval, bandbreedte of beide. In deze zelfstudie we zijn niet beperken op basis van bandbreedte, dus het kenmerk **bandbreedte** verwijderen.

    <quota calls="number" renewal-period="seconds">
    </quota>

In het product gratis proefversie wordt het quotum 200 oproepen per week. **200** opgeven als de waarde voor het kenmerk **oproepen** en geef **604800** als de waarde voor de **verlenging-punt** -kenmerk.

    <quota calls="200" renewal-period="604800">
    </quota>

>Beleid intervallen zijn opgegeven in seconden. Als u wilt berekenen het interval voor een week, kunt u het aantal dagen (7) met het aantal uren per dag (24) door het aantal minuten in een uur (60) met het aantal seconden in minuten (60) vermenigvuldigen: 7 *24* 60 * 60 = 604800.

Wanneer u klaar bent met het configureren van het beleid, moet deze overeenkomen met het volgende voorbeeld.

    <policies>
        <inbound>
            <rate-limit calls="10" renewal-period="60">
            </rate-limit>
            <quota calls="200" renewal-period="604800">
            </quota>
            <base />

    </inbound>
    <outbound>

        <base />

        </outbound>
    </policies>

Nadat u het gewenste beleid zijn geconfigureerd, klikt u op **Opslaan**.

![Beleid opslaan][api-management-policy-save]

## <a name="publish-product"></a> Het product publiceren

Nu de de API's worden toegevoegd en het beleid wordt geconfigureerd, het product moet worden gepubliceerd, zodat deze kan worden gebruikt door ontwikkelaars. **Producten** in het menu **API-beheer** aan de linkerkant op en klik vervolgens op **Gratis proefversie** om te configureren van het product.

![Product configureren][api-management-configure-product]

Klik op **publiceren**en klik vervolgens op **Ja, project publiceren** om te bevestigen.

![Product publiceren][api-management-publish-product]

## <a name="subscribe-account"> </a>Wilt abonneren op een ontwikkelaarsaccount tot het product

Nu dat het product wordt gepubliceerd, is dit is beschikbaar voor geabonneerd op en worden gebruikt door ontwikkelaars.

>Beheerders van een exemplaar API Management worden automatisch geabonneerd op elk product. In deze zelfstudie stap wordt we een van de accounts geen beheerder ontwikkelaars tot het product gratis proefversie abonneren. Als uw ontwikkelaarsaccount deel uit van de rol beheerders maakt, kunt klikt u vervolgens u volgen samen met deze stap, zelfs als u al bent geabonneerd.

Klik op **gebruikers** in het menu **API-beheer** aan de linkerkant en klik vervolgens op de naam van uw ontwikkelaarsaccount. In dit voorbeeld gebruiken we het **Cool Gragg** ontwikkelaars-account.

![Ontwikkelaars configureren][api-management-configure-developer]

Klik op **abonnement toevoegen**.

![Abonnement toevoegen][api-management-add-subscription-menu]

**Gratis proefversie**selecteren en klik vervolgens op **aanmelden**.

![Abonnement toevoegen][api-management-add-subscription]

>[AZURE.NOTE] In deze zelfstudie zijn meerdere tegelijk abonnementen niet ingeschakeld voor de gratis proefversie-product. Als deze zijn, zou u gevraagd naam opgeven voor het abonnement, zoals wordt weergegeven in het volgende voorbeeld.

![Abonnement toevoegen][api-management-add-subscription-multiple]

Nadat u hebt geklikt **abonneren**, is het product wordt weergegeven in de lijst met **abonnementen** voor de gebruiker.

![Abonnement toegevoegd][api-management-subscription-added]

## <a name="test-rate-limit"> </a>Om te bellen van een bewerking en testen van de limiet

We kunnen nu dat het product gratis proefversie is geconfigureerd en die zijn gepubliceerd, sommige bewerkingen bellen en het beleid tarief testen.
Overschakelen naar de developer portal door te klikken op **Developer portal** in het menu rechtsboven.

![Developer portal][api-management-developer-portal-menu]

**API's** in het bovenste menu op en klik vervolgens op **Echo API**.

![Developer portal][api-management-developer-portal-api-menu]

Klik op **Resource ophalen**en klik vervolgens op **het uitproberen**.

![Geopende console][api-management-open-console]

De standaard parameterwaarden behouden en selecteer uw abonnement-toets voor de gratis proefversie-product.

![Abonnement-toets][api-management-select-key]

>[AZURE.NOTE] Als u meerdere abonnementen hebt, controleert u of de sleutel voor **Gratis proefversie**, anders de beleidsregels die zijn geconfigureerd in de vorige stappen niet meer van kracht.

Klik op **verzenden**en vervolgens het antwoord te bekijken. Houd rekening met de **antwoordstatus** van **200 OK**.

![Resultaat van de bewerking][api-management-http-get-results]

Klik op **verzenden** met een groter is dan het beleid limiet van 10 oproepen per minuut snelheid. Nadat het beleid limiet is overschreden, is de antwoordstatus van een van **429 te veel aanvragen** als resultaat gegeven.

![Resultaat van de bewerking][api-management-http-get-429]

De **inhoud van de reactie** geeft aan dat het resterende interval voordat de nieuwe pogingen kunnen worden geverifieerd.

Wanneer het beleid limiet van 10 oproepen per minuut ingeschakeld is, mislukken volgende oproepen, mislukt tot 60 seconden na de eerste van de 10 succesvolle oproepen tot het product voordat de limiet is overschreden. In dit voorbeeld is het resterende interval 54 seconden.

## <a name="next-steps"> </a>Vervolgstappen

-   Bekijk een video van de limieten voor de snelheid en quota instellen in de volgende video.

> [AZURE.VIDEO rate-limits-and-quotas]


[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Uw eerste API in Azure API Management beheren]: api-management-get-started.md
[Het maken en hierin groepen wilt gebruiken in Azure API Management]: api-management-howto-create-groups.md
[View subscribers to a product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[Een exemplaar van de service API Management maken]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[Gesprek tarief quota en limiet beleidsregels configureren]: #policies
[Add an API to the product]: #add-api
[Publish the product]: #publish-product
[Subscribe a developer account to the product]: #subscribe-account
[Call an operation and test the rate limit]: #test-rate-limit

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota
