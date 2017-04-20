<properties 
    pageTitle="Het maken en publiceren van een product in Azure API Management" 
    description="Informatie over het maken en publiceren van producten in Azure API Management." 
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

# <a name="how-to-create-and-publish-a-product-in-azure-api-management"></a>Het maken en publiceren van een product in Azure API Management

Beheer van Azure API bevat een product een of meer API's, evenals een quota Resourcegebruik en de gebruiksvoorwaarden. Als een product is gepubliceerd, kunnen ontwikkelaars abonneren op het product en begint met het gebruik van het product API's. Het onderwerp biedt een handleiding voor het maken van een product, het toevoegen van een API en publiceren voor ontwikkelaars.

## <a name="create-product"> </a>Maken van een product

Bewerkingen zijn toegevoegd en geconfigureerd voor een API in de publisher-portal. Voor toegang tot de publisher-portal op **beheren** in de klassieke Azure-Portal voor uw service API Management.

![Publisher-portal][api-management-management-console]

>Als u een exemplaar van de service API Management nog niet hebt gemaakt, raadpleegt u [een exemplaar van de service API Management maken][] in deze zelfstudie [aan de slag met Azure API Management][] .

Klik op **producten** in het menu aan de linkerkant om weer te geven van de pagina **producten** , en klik op **Product toevoegen**.

![Producten][api-management-products]

![Nieuw product][api-management-add-new-product]

Voer een beschrijvende naam voor het product in het veld **naam** en een beschrijving van het product in het veld **Beschrijving** .

Producten in API Management kunnen worden **geopend** of **beveiligde**. Beveiligde producten moeten zijn geabonneerd op voordat ze kunnen worden gebruikt, terwijl openen producten zonder een abonnement kunnen worden gebruikt. Controleer **abonnement vereisen** om een beveiligde product waarvoor een abonnement te maken. Dit is de standaardinstelling.

Schakel **abonnement goedkeuring vereisen** als u een beheerder om te controleren en accepteren of negeren abonnement pogingen voor dit product wilt. Als het vak niet ingeschakeld is, zijn abonnement pogingen automatisch goedgekeurd. Zie voor meer informatie over abonnementen, [weergave abonnees van een product][].

Schakel het selectievakje **toestaan meerdere abonnementen** zodat ontwikkelaars accounts zich kunnen abonneren meerdere keren op het product. Als dit selectievakje niet is ingeschakeld, kunt u elk ontwikkelaarsaccount slechts één keer tot het product abonneren.

![Onbeperkte meerdere abonnementen][api-management-unlimited-multiple-subscriptions]

Als u wilt beperken het aantal van meerdere tegelijk abonnementen, schakel het selectievakje **aantal tegelijk abonnementen op beperken** in en voer de limiet van abonnement. Gelijktijdige abonnementen zijn in het volgende voorbeeld wordt beperkt tot vier per ontwikkelaarsaccount.

![Vier meerdere abonnementen][api-management-four-multiple-subscriptions]

Nadat alle nieuwe productopties zijn geconfigureerd, klikt u op **Opslaan** om het nieuwe product te maken.

![Producten][api-management-products-page]

>Standaard nieuwe producten zijn niet-gepubliceerde en zijn alleen zichtbaar voor de groep **Administrators** .

Als u wilt een product configureren, klik op de naam van het product op het tabblad **producten** .

## <a name="add-apis"> </a>API's toevoegen aan een product

De pagina **producten** bevat vier koppelingen voor configuratie: **Samenvatting**, **Instellingen**, **zichtbaarheid**en **abonnees**. Het tabblad **Samenvatting** is waar u kunt API's toevoegen en publiceren of intrekken van een product.

![Overzicht][api-management-new-product-summary]

Alvorens uw product dat u wilt toevoegen van een of meer API's. Klik op **Toevoegen API product**hiervoor.

![API's toevoegen][api-management-add-apis-to-product]

Selecteer de gewenste API's en klik op **Opslaan**.

## <a name="add-description"> </a>Beschrijvende informatie toevoegen aan een product

Het tabblad **Instellingen** kunt u gedetailleerde informatie over het product zoals het doel, de deze toegang tot biedt-API's en andere nuttige informatie te verstrekken. De inhoud is gericht op de ontwikkelaars die bellen van de API en kunnen worden geschreven in tekst zonder opmaak of HTML-opmaak.

![Product-instellingen][api-management-product-settings]

Schakel **vereisen abonnement** als u wilt maken van een beveiligde product waarvoor een abonnement moet worden gebruikt of schakel het selectievakje uit als u wilt maken van een open product dat kan worden aangeroepen zonder een abonnement.

Selecteer het **abonnement goedkeuring vereisen** als u wilt alle aanvragen voor het abonnement van product handmatig goedkeuren. Standaard worden automatisch alle product abonnementen verleend.

Schakel het selectievakje **toestaan dat meerdere abonnementen** in zodat ontwikkelaars accounts zich kunnen abonneren meerdere keren op het product en opgeven (optioneel) een beperking. Als dit selectievakje niet is ingeschakeld, kunt u elk ontwikkelaarsaccount slechts één keer tot het product abonneren.

(Optioneel) Voer de **Gebruiksvoorwaarden** -veld met een beschrijving van de gebruiksvoorwaarden voor het product welke abonnees accepteren moeten om te kunnen gebruiken van het product.

## <a name="publish-product"> </a>Publiceren van een product

Voordat de API's in een product kan worden aangeroepen, kan het product moet worden gepubliceerd. Klik op **publiceren**op het tabblad **Overzicht** voor het product en klik vervolgens op **Ja, project publiceren** om te bevestigen. Een eerder gepubliceerde product als privé wilt maken, klikt u op **publicatie ongedaan maken**.

![Product publiceren][api-management-publish-product]

## <a name="make-visible"> </a>Een product zichtbaar maken voor ontwikkelaars

Het tabblad **zichtbaarheid** kunt u kiezen welke functies zijn kunnen zien van het product op de developer portal en abonneren op het product.

![Product zichtbaarheid][api-management-product-visiblity]

Als u wilt in- of uitschakelen van zichtbaarheid van een product voor ontwikkelaars van een groep, controleren of schakel het selectievakje naast de groep en klik vervolgens op **Opslaan**.

>Lees [hoe u maken en gebruiken van groepen om ontwikkelaars accounts in Azure API Management beheren][]voor meer informatie.

## <a name="view-subscribers"> </a>Abonnees van een product weergeven

Het tabblad **abonnees** bevat de ontwikkelaars die zich hebt aangemeld bij het product. De details en instellingen voor elke ontwikkelaars kunnen worden bekeken door te klikken op de naam van de ontwikkelaar. In dit voorbeeld hebben nog geen ontwikkelaars geabonneerd op het product.

![Ontwikkelaars][api-management-developer-list]

## <a name="next-steps"> </a>Vervolgstappen

Zodra de gewenste API's worden toegevoegd en het product is gepubliceerd, ontwikkelaars kunnen abonneren op het product en beginnen met de API's bellen. Zie [hoe maken en configureren van geavanceerde product-instellingen in Azure API Management][]voor een zelfstudie waarop u deze items, evenals de geavanceerde configuratie.

Zie de volgende video voor meer informatie over het werken met producten.

> [AZURE.VIDEO using-products]

[Create a product]: #create-product
[Add APIs to a product]: #add-apis
[Add descriptive information to a product]: #add-description
[Publish a product]: #publish-product
[Make a product visible to developers]: #make-visible
[Weergave abonnees van een product]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[Aan de slag met Azure API Management]: api-management-get-started.md
[Een exemplaar van de service API Management maken]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[Het maken en hierin groepen wilt gebruiken voor het beheren van ontwikkelaars accounts in het beheer van Azure-API]: api-management-howto-create-groups.md
[Hoe maken en configureren van geavanceerde product-instellingen in Azure API Management]: api-management-howto-product-with-rules.md 