<properties 
    pageTitle="Het aanpassen van de Azure API Management developer portal met sjablonen | Microsoft Azure" 
    description="Informatie over het aanpassen van de Azure API Management developer portal met sjablonen." 
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


# <a name="how-to-customize-the-azure-api-management-developer-portal-using-templates"></a>Het aanpassen van de Azure API Management developer portal met sjablonen

Azure API Management bevat diverse nieuwe aanpassingsfuncties kunnen beheerders [het uiterlijk van de developer portal aanpassen](api-management-customize-portal.md), evenals de inhoud van de developer portal's met behulp van een set van sjablonen die configureren van de inhoud van de pagina's zelf aanpassen. De syntaxis van de [DotLiquid](http://dotliquidmarkup.org/) en een opgegeven aantal gelokaliseerde tekenreeks resources, pictogrammen en paginabesturingselementen gebruikt, hebt u uitstekende flexibiliteit voor het configureren van de inhoud van de pagina's naar wens met deze sjablonen.

## <a name="developer-portal-templates-overview"></a>Overzicht van Developer portal-sjablonen

Developer portal sjablonen worden beheerd in de developer portal door beheerders van het exemplaar van de service API Management. Als u wilt beheren ontwikkelaars sjablonen, Ga naar uw exemplaar van de service API Management in de klassieke Azure-Portal en klik op **Bladeren**.

![Developer portal][api-management-browse]

Als u al in de publisher-portal bent, kunt u de developer portal openen door te klikken op **Developer portal**.

![Developer portal-menu][api-management-developer-portal-menu]

Voor toegang tot de portal-sjablonen voor ontwikkelaars, klik op het pictogram aanpassen aan de linkerkant het aanpassen van menu weergeven en klikt u op **sjablonen**.

![Developer portal-sjablonen][api-management-customize-menu]

De lijst met sjablonen worden verschillende categorieën met sjablonen die betrekking hebben op de verschillende pagina's in de portal voor ontwikkelaars. Elke sjabloon wordt weergegeven, maar de stappen voor het bewerken en publiceer de wijzigingen zijn hetzelfde. Als u wilt bewerken van een sjabloon, klikt u op de naam van de sjabloon.

![Developer portal-sjablonen][api-management-templates-menu]

Te klikken op een sjabloon Hiermee, gaat u naar de ontwikkelaars-portalpagina die kan worden aangepast die sjabloon. In dit voorbeeld de **lijst met producten** wordt sjabloon weergegeven. Het gebied van het scherm dat wordt aangegeven door de rode rechthoek Hiermee stelt u de sjabloon **lijst met producten** . 

![De lijstsjabloon producten][api-management-developer-portal-templates-overview]

Enkele sjablonen, zoals de sjablonen **Gebruikersprofiel** aanpassen verschillende onderdelen van dezelfde pagina. 

![Profiel gebruikerssjablonen][api-management-user-profile-templates]

De editor voor elke developer portal sjabloon bevat twee secties weergegeven onder aan de pagina. De linkerkant worden weergegeven het deelvenster bewerken voor de sjabloon en de rechterkant het gegevensmodel voor de sjabloon wordt weergegeven. 

De sjabloon voor het bewerken van deelvenster bevat de markeringen die het uiterlijk en gedrag van de bijbehorende pagina in de developer portal bepaalt. De opmaak van de sjabloon worden de syntaxis van de [DotLiquid](http://dotliquidmarkup.org/) gebruikt. Eén populaire editor voor DotLiquid is [DotLiquid voor ontwerpers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers). Eventuele wijzigingen aan de sjabloon tijdens het bewerken in realtime worden weergegeven in de browser, maar zijn niet zichtbaar voor uw klanten totdat u [Opslaan](#to-save-a-template) en [publiceren](#to-publish-a-template) de sjabloon.

![Sjabloon markeringen][api-management-template]

Het deelvenster **gegevens van de sjabloon** biedt een handleiding voor het gegevensmodel voor de entiteiten die beschikbaar voor gebruik in een bepaalde sjabloon zijn. Deze handleiding krijgen door de persoonlijke gegevens die momenteel worden weergegeven in de developer portal weer te geven. U kunt de deelvensters sjabloon uitvouwen door te klikken op de rechthoek in de rechterbovenhoek van het deelvenster **gegevens van de sjabloon** .

![Sjabloon-gegevensmodel][api-management-template-data]

In het vorige voorbeeld zijn er twee producten in de developer portal weergegeven die zijn opgehaald uit de gegevens worden weergegeven in het deelvenster **gegevens van de sjabloon** , zoals wordt weergegeven in het volgende voorbeeld.

    {
        "Paging": {
            "Page": 1,
            "PageSize": 10,
            "TotalItemCount": 2,
            "ShowAll": false,
            "PageCount": 1
        },
        "Filtering": {
            "Pattern": null,
            "Placeholder": "Search products"
        },
        "Products": [
            {
                "Id": "56ec64c380ed850042060001",
                "Title": "Starter",
                "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",
                "Terms": "",
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            },
            {
                "Id": "56ec64c380ed850042060002",
                "Title": "Unlimited",
                "Description": "Subscribers have completely unlimited access to the API. Administrator approval is required.",
                "Terms": null,
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            }
        ]
    }

De opmaak in de **lijst met producten** sjabloon verwerkt de gegevens voor de gewenste uitvoer door te doorlopen van de siteverzameling van producten weer te geven informatie en een koppeling voor elk afzonderlijk product. Opmerking de `<search-control>` en `<page-control>` elementen in de markeringen. Deze de weergave bepalen van de zoek- en paginering besturingselementen op de pagina. `ProductsStrings|PageTitleProducts`een verwijzing is gelokaliseerde tekenreeks met de `h2` koptekst voor de pagina. Voor een lijst met tekenreeks resources, paginabesturingselementen en pictogrammen voor developer portal sjablonen gebruiken, raadpleegt u [Naslaginformatie voor ontwikkelaars portal sjablonen API Management](https://msdn.microsoft.com/library/azure/mt697540.aspx).

    <search-control></search-control>
    <div class="row">
        <div class="col-md-9">
            <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>
        </div>
    </div>
    <div class="row">
        <div class="col-md-12">
        {% if products.size > 0 %}
        <ul class="list-unstyled">
        {% for product in products %}
            <li>
                <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>
                {{product.description}}
            </li>   
        {% endfor %}
        </ul>
        <paging-control></paging-control>
        {% else %}
        {% localized "CommonResources|NoItemsToDisplay" %}
        {% endif %}
        </div>
    </div>

## <a name="to-save-a-template"></a>Een sjabloon opslaan

Als u wilt een sjabloon opslaan, klik op opslaan in de sjablooneditor.

![Sjabloon opslaan][api-management-save-template]

Opgeslagen wijzigingen zijn niet in de developer portal leven totdat ze worden gepubliceerd.

## <a name="to-publish-a-template"></a>Publiceren van een sjabloon

Opgeslagen sjablonen kunnen worden gepubliceerd individueel, of allemaal tegelijk. Als u wilt publiceren op een afzonderlijke sjabloon, klik op publiceren in de sjablooneditor.

![Publicatiesjabloon][api-management-publish-template]

Klik op **Ja** om te bevestigen en controleer de sjabloon live op de developer portal.

![Bevestig publiceren][api-management-publish-template-confirm]

Als u wilt publiceren alle momenteel niet-gepubliceerde sjabloonversies, klik op **publiceren** in de lijst met sjablonen. Niet-gepubliceerde sjablonen zijn gemarkeerd met een sterretje achter de sjabloonnaam. In dit voorbeeld worden de **lijst met producten** en de **productcode** sjablonen gepubliceerd.

![Sjablonen publiceren][api-management-publish-templates]

Klik op **publiceren aanpassingen** om te bevestigen.

![Bevestig publiceren][api-management-publish-customizations]

Nieuwe sjablonen zijn effectieve onmiddellijk in de portal voor ontwikkelaars.

## <a name="to-revert-a-template-to-the-previous-version"></a>Een sjabloon naar de vorige versie herstellen

Als u wilt terugkeren naar de vorige gepubliceerde versie sjabloon, klikt u op teruggezet in de sjablooneditor.

![Herstel van sjabloon][api-management-revert-template]

Klik op **Ja** om te bevestigen.

![Bevestigen][api-management-revert-template-confirm]

De eerder gepubliceerde versie van een sjabloon is live in de developer portal zodra de bewerking omkeren voltooid is.

## <a name="to-restore-a-template-to-the-default-version"></a>Een sjabloon naar de standaardversie herstellen

Sjablonen herstellen naar de standaardversie is een proces. Eerst de sjablonen moeten worden teruggezet en klikt u vervolgens de herstelde versies moeten worden gepubliceerd.

Klik als u wilt herstellen, een enkele sjabloon naar de standaardversie op terugzetten in de sjablooneditor.

![Herstel van sjabloon][api-management-reset-template]

Klik op **Ja** om te bevestigen.

![Bevestigen][api-management-reset-template-confirm]

Als u alle sjablonen herstellen naar een standaard-versie, klikt u op **standaardsjablonen herstellen** in de lijst met sjablonen.

![Sjablonen herstellen][api-management-restore-templates]

De herstelde sjablonen moeten vervolgens worden gepubliceerd afzonderlijk of allemaal tegelijk volgens de stappen in het [publiceren van een sjabloon](#to-publish-a-template).

## <a name="developer-portal-templates-reference"></a>Naslaginformatie voor ontwikkelaars portal-sjablonen

Zie [Naslaginformatie voor ontwikkelaars portal sjablonen API Management](https://msdn.microsoft.com/library/azure/mt697540.aspx)voor naslaginformatie voor ontwikkelaars portal sjablonen, tekenreeks resources, pictogrammen en paginabesturingselementen.

## <a name="watch-a-video-overview"></a>Bekijk een video-overzicht

Bekijk de volgende video om te zien hoe een discussiebord en classificaties toevoegen aan de API en bewerking pagina in de developer portal met sjablonen.

> [AZURE.VIDEO adding-developer-portal-functionality-using-templates-in-azure-api-management]


[api-management-customize-menu]: ./media/api-management-developer-portal-templates/api-management-customize-menu.png
[api-management-templates-menu]: ./media/api-management-developer-portal-templates/api-management-templates-menu.png
[api-management-developer-portal-templates-overview]: ./media/api-management-developer-portal-templates/api-management-developer-portal-templates-overview.png
[api-management-template]: ./media/api-management-developer-portal-templates/api-management-template.png
[api-management-template-data]: ./media/api-management-developer-portal-templates/api-management-template-data.png
[api-management-developer-portal-menu]: ./media/api-management-developer-portal-templates/api-management-developer-portal-menu.png
[api-management-browse]: ./media/api-management-developer-portal-templates/api-management-browse.png
[api-management-user-profile-templates]: ./media/api-management-developer-portal-templates/api-management-user-profile-templates.png
[api-management-save-template]: ./media/api-management-developer-portal-templates/api-management-save-template.png
[api-management-publish-template]: ./media/api-management-developer-portal-templates/api-management-publish-template.png
[api-management-publish-template-confirm]: ./media/api-management-developer-portal-templates/api-management-publish-template-confirm.png
[api-management-publish-templates]: ./media/api-management-developer-portal-templates/api-management-publish-templates.png
[api-management-publish-customizations]: ./media/api-management-developer-portal-templates/api-management-publish-customizations.png
[api-management-revert-template]: ./media/api-management-developer-portal-templates/api-management-revert-template.png
[api-management-revert-template-confirm]: ./media/api-management-developer-portal-templates/api-management-revert-template-confirm.png
[api-management-reset-template]: ./media/api-management-developer-portal-templates/api-management-reset-template.png
[api-management-reset-template-confirm]: ./media/api-management-developer-portal-templates/api-management-reset-template-confirm.png
[api-management-restore-templates]: ./media/api-management-developer-portal-templates/api-management-restore-templates.png







