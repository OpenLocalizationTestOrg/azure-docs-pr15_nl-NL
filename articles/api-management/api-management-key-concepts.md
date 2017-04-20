<properties 
    pageTitle="Beheer van de API-basisbegrippen" 
    description="Meer informatie over API's, producten, rollen, groepen en andere belangrijke concepten API Management." 
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
    ms.topic="hero-article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

#<a name="what-is-api-management"></a>Wat is API Management?

API Management helpt organisaties publiceren API's om externe, partner en interne ontwikkelaars als u wilt ontgrendelen van de mogelijkheden van hun gegevens en -services. Bedrijven zijn overal wilt uitbreiden van hun activiteiten als een digitale platform, maken van nieuwe kanalen, zoek naar nieuwe klanten en sturende grondigere betrokkenheid met bestaande ideeën. API-beheer biedt de bevoegdheden core om ervoor te zorgen een succesvolle API-programma via ontwikkelaars betrokkenheid, bedrijven inzichten, analyses, beveiliging en beveiliging.

Bekijk de volgende video voor een overzicht van Azure API Management en informatie over het gebruik van API Management veel functies toevoegen aan uw API, met inbegrip van toegangsbeheer, rente beperken, controle, logboekregistratie en caching van antwoord, met minimale werk op uw onderdeel.

> [AZURE.VIDEO azure-api-management-overview]

Beheerders maken als u wilt gebruiken API Management, API's. Elke API bestaat uit een of meer bewerkingen en elke API kan worden toegevoegd aan een of meer producten. Als u een API, ontwikkelaars abonneren op een product dat API met, en vervolgens deze bewerking voor de API's, onderhevig aan eventuele gebruiksbeleid die mogelijk van kracht kunnen bellen.

In dit onderwerp biedt een overzicht van belangrijke concepten API Management.

>[AZURE.NOTE] Zie voor meer informatie de [cloudgebaseerde API Management: gebruik te maken van de Power van API's](http://j.mp/ms-apim-whitepaper) PDF-whitepaper. Deze inleidende whitepaper over API-beheer door CITO Research worden de volgende onderwerpen besproken: 
>
> - Algemene vereisten voor API en uitdagingen
>     - Ontkoppeling API's en facades presenteren
>     - Ontwikkelaars slag en snel aan de slag
>     - Toegang beveiligen
>     - Analyses en aan de doelstellingen
> - Controle over en inzicht met een platform-API Management krijgen
> - Cloud tegenover on-premises-oplossingen
> - Azure API Management

## <a name="apis"> </a>API's en bedrijfsactiviteiten

API's zijn de basis van een exemplaar van de service API Management. Elke API vertegenwoordigt een reeks bewerkingen die beschikbaar zijn voor ontwikkelaars. Elke API bevat een verwijzing naar de back-enddatabase-service die de API implementeert en de bewerkingen toewijzen aan de acties van de back-enddatabase-service. Bewerkingen in API Management worden ten zeerste geconfigureerd, met controle over URL-toewijzing, query en pad parameters aanvraag en antwoord inhoud en caching van bewerking antwoord. Limiet, quota's en IP-beperking beleidsregels kunnen ook worden geïmplementeerd op de API of het niveau van de afzonderlijke bewerking.

Zie [hoe API's maken][] en [hoe u bewerkingen tot een API toevoegen][]voor meer informatie.


## <a name="products"></a> Producten

Producten zijn hoe API's voor ontwikkelaars zijn opgehaald. Producten in API Management een of meer API's hebt en zijn geconfigureerd met een titel, beschrijving en gebruiksvoorwaarden. Producten kunnen worden **geopend** of **beveiligde**. Beveiligde producten moeten zijn geabonneerd op voordat ze kunnen worden gebruikt, terwijl openen producten zonder een abonnement kunnen worden gebruikt. Wanneer u een product is gereed voor gebruik door ontwikkelaars kunnen worden gepubliceerd. Nadat deze is gepubliceerd, deze kan worden weergegeven (en beveiligde producten geabonneerd op) door ontwikkelaars. Goedkeuring van een abonnement kunt op het productniveau van het is geconfigureerd en u Goedkeuring beheerder vereisen of worden automatisch goedgekeurd.

Groepen worden gebruikt voor het beheren van de zichtbaarheid van producten voor ontwikkelaars. Producten zichtbaarheid verlenen aan groepen en ontwikkelaars kunnen bekijken en abonneren op de producten die zichtbaar zijn voor de groepen waarin ze behoren. 

Zie [hoe maken en publiceren van een product][] en de volgende video voor meer informatie.

> [AZURE.VIDEO using-products]

## <a name="groups"></a> Groepen

Groepen worden gebruikt voor het beheren van de zichtbaarheid van producten voor ontwikkelaars. API Management heeft de volgende onveranderlijke systeemgroepen.

-   **Beheerders** - Azure abonnement beheerders zijn leden van deze groep. Beheerders beheren API Management service-exemplaren, de API's, bewerkingen en producten die worden gebruikt door ontwikkelaars maken.
-   **Ontwikkelaars** - geverifieerde developer portal gebruikers in deze groep vallen. Ontwikkelaars zijn de klanten naar wie u uw API's met toepassingen bouwen. Ontwikkelaars toegang krijgen tot de developer portal en toepassingen maken die de bewerkingen van een API bellen.
-   **Gasten** - portal niet-geverifieerde ontwikkelaars-gebruikers, zoals potentiële klanten die u bezoekt de developer portal een exemplaar van de API Management vallen in deze groep. Ze kunnen toegang worden verleend bepaalde alleen-lezen, zoals de mogelijkheid API's weergeven, maar deze niet te bellen.

Beheerders kunnen naast deze systeemgroepen aangepaste groepen of [gebruikmaken van externe groepen in de bijbehorende Azure Active Directory-tenants](api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group)maken. Aangepaste en externe groepen kunnen worden gebruikt samen met de systeemgroepen in het ontwikkelaars zichtbaarheid en toegang verlenen aan API-producten. U kunt bijvoorbeeld een aangepaste groep maken voor ontwikkelaars die zijn verbonden met een specifieke partner-organisatie en hen in staat stellen toegang tot de API's van een product dat relevante API's bevat. Een gebruiker kan een lid zijn van meer dan één groep zijn.

Lees [hoe u maken en hierin groepen wilt gebruiken][]voor meer informatie.

## <a name="developers"></a> Ontwikkelaars

Ontwikkelaars vertegenwoordigen de gebruikersaccounts in een exemplaar van de service API Management. Ontwikkelaars kunnen worden gemaakt of uitgenodigd deel te nemen aan door beheerders, of ze kunnen registreren in de [portal voor ontwikkelaars][]. Elke ontwikkelaars deel uitmaakt van een of meer groepen en kan zijn abonneren op de producten die zichtbaarheid aan deze groepen verlenen.

Als ontwikkelaars zich op een product abonneert krijgen ze de primaire en secundaire sleutel voor het product. Deze toets wordt gebruikt bij het plaatsen van oproepen naar API's van het product.

Zie [hoe u maakt of ontwikkelaars uitnodigen][] en [hoe u groepen met ontwikkelaars koppelen][]voor meer informatie.

## <a name="policies"></a> Beleid

Beleid is een krachtige videomogelijkheden van API Management waarmee de uitgever die u wilt wijzigen van het gedrag van de API tot en met de configuratie. Beleidsregels zijn antwoord van een API of een verzameling instructies die opeenvolging op het verzoek worden uitgevoerd. Populaire overzichten bevatten conversie van bestandsindeling van XML-JSON en gesprek tarief beperken voor het beperken van de hoeveelheid binnenkomende oproepen van een ontwikkelaar en vele andere machtigingen zijn beschikbaar.

Beleid expressies kunnen worden gebruikt als kenmerkwaarden of tekst in een van de API beleidsregels voor, tenzij anderszins Hiermee geeft u het beleid. Bepaalde beleidsinstellingen zoals het beleid [besturingsstroom](https://msdn.microsoft.com/library/azure/dn894085.aspx#choose) en [stelt u variabele](https://msdn.microsoft.com/library/azure/dn894085.aspx#set-variable) zijn gebaseerd op beleid expressies. Zie [Geavanceerde beleidsregels](https://msdn.microsoft.com/library/azure/dn894085.aspx#AdvancedPolicies), [beleid expressies](https://msdn.microsoft.com/library/azure/dn910913.aspx), en bekijk de volgende video voor meer informatie.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

Zie voor een volledige lijst van beleidsregels voor informatiebeheer API, [beleidsverwijzing][]. Zie voor meer informatie over het gebruik en beleid configureren [API beheerbeleid][]. Zie voor een zelfstudie over het maken van een product met tarief quota en limiet beleidsregels, [hoe maken en configureren van geavanceerde product-instellingen][]. Zie de volgende video voor een video.

> [AZURE.VIDEO rate-limits-and-quotas]

## <a name="developer-portal"></a> Developer portal

De developer portal is waar ontwikkelaars kunnen meer informatie over uw bewerkingen API's, de weergave en de oproep en u hierop abonneren producten. Potentiële klanten kunnen Help.Yammer de developer portal, API's en bewerkingen weergeven en registreren. De URL voor de portal ontwikkelaars bevindt zich op het dashboard in de klassieke Azure-Portal voor uw exemplaar van de service API Management.

U kunt het uiterlijk van uw developer portal aanpassen door aangepaste inhoud toe te voegen, stijlen aan te passen en uw huisstijl toe te voegen.

## <a name="api-management-and-the-api-economy"></a>API Management en de economie API

Meer informatie over API Management, bekijk de volgende presentatie uit de vergadering Microsoft Ignite 2015.

> [AZURE.VIDEO microsoft-ignite-2015-azure-api-management-and-the-api-economy]

[APIs and operations]: #apis
[Products]: #products
[Groups]: #groups
[Developers]: #developers
[Policies]: #policies
[Developer portal]: #developer-portal

[Het maken van API 's]: api-management-howto-create-apis.md
[Bewerkingen toevoegen aan een API]: api-management-howto-add-operations.md
[Het maken en publiceren van een product]: api-management-howto-add-products.md
[Het maken en hierin groepen wilt gebruiken]: api-management-howto-create-groups.md
[Het koppelen van groepen met ontwikkelaars]: api-management-howto-create-groups.md#associate-group-developer
[Hoe maken en configureren van geavanceerde product-instellingen]: api-management-howto-product-with-rules.md
[Het maken of ontwikkelaars uitnodigen]: api-management-howto-create-or-invite-developers.md
[Beleidsverwijzing]: api-management-policy-reference.md
[Beleidsregels voor informatiebeheer API]: api-management-howto-policies.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance



 
