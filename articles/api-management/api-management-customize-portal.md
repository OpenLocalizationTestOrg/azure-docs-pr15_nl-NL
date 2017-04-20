<properties
    pageTitle="Aanpassen van de portal voor ontwikkelaars in Azure API Management | Microsoft Azure"
    description="Informatie over het aanpassen van de portal voor ontwikkelaars in Azure API Management."
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

# <a name="customize-the-developer-portal-in-azure-api-management"></a>De developer portal in Azure API Management aanpassen

Deze handleiding ziet u hoe u het uiterlijk van de portal voor ontwikkelaars in Azure API Management voor consistentie met uw huisstijl wijzigen.

## <a name="change-page-headers"> </a>De tekst of het logo in koptekst van de pagina wijzigen

Een van de belangrijkste aspecten van de portal aanpassing is de tekst boven aan alle pagina's vervangen door uw bedrijfsnaam of logo.

Inhoud binnen de developer portal is via de publisher-portal, die wordt geopend met de klassieke Azure-Portal gewijzigd. Klik op **beheren** in de klassieke Azure-Portal voor uw API Management-service te bereiken de API publisher-portal.

![Publisher-portal][api-management-management-console]

De developer portal is gebaseerd op een inhoudsbeheersysteem of CMS. De koptekst die wordt weergegeven op elke pagina is een speciaal type inhoud bekend als een object. Als u wilt bewerken in de inhoud van deze widget, **Widgets** op in het menu **Developer Portal** aan de linkerkant en selecteer vervolgens de widget **kop** in de lijst.

![Widgets koptekst][api-management-widgets-header]

De inhoud van de koptekst kan worden bewerkt uit in het veld **hoofdtekst** . Wijzig de tekst in "Fabrikam Developer Portal" en klik vervolgens op **Opslaan** onder aan de pagina.

Nu moet u mogelijk om de nieuwe kop op elke pagina binnen de developer portal weer te geven.

> Als u wilt de developer portal terwijl u in de publisher-portal openen, klikt u op **Developer portal** op de bovenste balk.

## <a name="change-headers-styling"> </a>De stijl van de koppen wijzigen

De kleuren, lettertypen, tekengrootten, afstanden en andere stijl-gerelateerde elementen van een willekeurige pagina op de portal worden gedefinieerd door opmaakregels. Als u wilt bewerken de stijlen, **uiterlijk** op in het menu **Developer portal** in de publisher-portal en klikt u op **begint aanpassen** zodat de opmaak-editor.

Uw browser schakelt u naar een verborgen pagina binnen de developer portal die voorbeelden van inhoud, met voorbeelden voor alle regels voor de opmaak gebruikt een willekeurige plaats op de site bevat. Als u wilt openen de vormgeving-editor, plaats u de cursor op de dunne grijze verticale lijn op de meest linkse deel van de pagina. De werkbalk editor moet worden weergegeven.

![De werkbalk aanpassen][api-management-customization-toolbar]

Er zijn twee belangrijkste manieren vormgeving regels bewerken: **alle regels bewerken** bevat een overzicht van alle stijl regels die worden gebruikt in een willekeurige plaats, terwijl **Kies element** Hiermee u een element selecteren uit de pagina die u kunt op en stijlen alleen voor dat element weergegeven.

In deze sectie willen we de stijl van alleen de berichtkoppen wijzigen. Klik op de optie **Kies element** van de werkbalk Opmaak-editor, en klik op **Selecteer een element om aan te passen**. Elementen worden nu gemarkeerd terwijl u de muisaanwijzer erboven met de muis om aan te geven welke grafiekelementstijlen u met het bewerken beginnen wilt als u hebt geklikt. Beweeg de muis over de tekst die de naam van het bedrijf in de koptekst ("Fabrikam Developer Portal' als u de instructies in de vorige sectie hebt gevolgd) en klikt u vervolgens op. Een reeks benoemde en gecategoriseerde vormgeving regels binnen de editor stijlen wordt weergegeven.

Elke regel vertegenwoordigt een eigenschap vormgeving van het geselecteerde element. Voor de koptekst hierboven hebt geselecteerd, de grootte van de tekst is bijvoorbeeld @font-size-h1 terwijl de naam van het lettertype met alternatieven wordt @headings-font-family.

> Als u bekend met [bootstrap bent][], zijn deze regels in feite [minder variabelen][] binnen het bootstrap thema gebruikt door de developer portal.

Laten we de kleur van de koptekst wijzigen. Selecteer het item in de **@headings-color** veld en typ **#000000**. Dit is de hexadecimale code voor de kleur zwart. Als u dit doet, ziet u dat een vierkant kleur-indicator wordt weergegeven aan het einde van het tekstvak. Als u op deze indicator klikt, wordt er een kleurenkiezer kunt u als u een kleur.

![Paletten Themakleuren en standaardkleuren][api-management-customization-toolbar-color-picker]

Wanneer u klaar bent met het aanbrengen van wijzigingen aan in de stijlen van het geselecteerde element, klikt u op **Voorbeeld van wijzigingen** in het scherm om de resultaten. Op dit moment kan zijn ze alleen zichtbaar voor beheerders. Als u deze wijzigingen zichtbaar voor iedereen, klikt u op de knop **publiceren** in de editor vormgeving en de wijzigingen te bevestigen.

![Menu publiceren][api-management-customization-toolbar-publish-form]

> Voer dezelfde procedure uit als u wilt wijzigen welke regels stijl op een ander element op de pagina toepassen, zoals voor de kop. Klik op **Kies een element** van de vormgeving-editor, selecteert u het element dat u geïnteresseerd bent en begint met het wijzigen van de waarden van de stijl regels op het scherm weergegeven.

## <a name="edit-page-contents"> </a>De inhoud van een pagina bewerken

De developer portal bestaat uit de automatisch gegenereerde pagina's zoals API's, producten, -toepassingen, problemen en handmatig geschreven inhoud. Omdat deze is gebaseerd op een inhoudsbeheersysteem, kunt u deze inhoud zo nodig kunt maken.

De lijst van alle bestaande inhoudspagina's wilt bekijken, klikt u op de **inhoud** in het menu **Developer portal** in de publisher-portal.

![Inhoud beheren][api-management-customization-manage-content]

Klik op **de welkomstpagina als u wilt bewerken op de startpagina van de portal ontwikkelaars wordt weergegeven** . Breng de wijzigingen die u wilt deze indien nodig een voorbeeld bekijken en klik vervolgens op **Nu publiceren** zodat ze zichtbaar voor iedereen.

> De startpagina van Lotus gebruikmaakt van een speciale indeling waarmee deze naar een banner boven weergeven. Deze banner kan niet worden bewerkt in de sectie met **inhoud** . Deze banner bewerken en klikt u op **objecten** in het menu **Developer portal** , selecteer **startpagina** in de vervolgkeuzelijst **Huidige laag** , opent u het item **Banner** onder de **sectie met aanbevolen**. De inhoud van deze widget kunnen worden bewerkt net als een andere pagina.

## <a name="next-steps"> </a>Vervolgstappen

-   Informatie over het aanpassen van de inhoud van developer portal-pagina's met [Developer portal sjablonen](api-management-developer-portal-templates.md).

[Change the text/logo in the page headers]: #change-page-headers
[Change the styling of the headers]: #change-headers-styling
[Edit the contents of a page]: #edit-page-contents
[Next steps]: #next-steps

[Azure Classic Portal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-customize-portal/api-management-management-console.png
[api-management-widgets-header]: ./media/api-management-customize-portal/api-management-widgets-header.png
[api-management-customization-toolbar]: ./media/api-management-customize-portal/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-customize-portal/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-customize-portal/api-management-customization-toolbar-publish-form.png
[api-management-customization-manage-content]: ./media/api-management-customize-portal/api-management-customization-manage-content.png


[bootstrap]: http://getbootstrap.com/
[MINDER variabelen]: http://getbootstrap.com/css/
