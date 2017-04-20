<properties
    pageTitle="Vooraf geladen activa op een eindpunt Azure CDN | Microsoft Azure"
    description="Leer hoe u vooraf laden in de cache opgeslagen inhoud op een CDN-eindpunt."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Vooraf geladen activa op een Azure CDN-eindpunt

[AZURE.INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Standaard activa eerst in cache opslaan als daarom wordt gevraagd. Dit betekent dat het eerste verzoek uit elke regio kan het langer duren, aangezien de edge-servers hebben geen de inhoud in cache opgeslagen en moet de aanvraag doorsturen naar de oorspronkelijke server. Inhoud vooraf geladen, wordt deze eerste treffers latentie voorkomen.

Naast een grotere klanttevredenheid, kunt vooraf geladen uw activa in de cache ook verkleinen netwerkverkeer op de oorspronkelijke server.

> [AZURE.NOTE] Vooraf laden activa is handig voor grote gebeurtenissen of inhoud die tegelijk beschikbaar voor een groot aantal gebruikers, zoals een nieuwe versie van de film of een software-update.

Deze zelfstudie leert u vooraf laden in de cache van de inhoud van alle Azure CDN rand knooppunten.

## <a name="walkthrough"></a>Stapsgewijze instructies

1. Blader naar het CDN-profiel met het eindpunt dat u wilt laden vooraf in de [Portal van Azure](https://portal.azure.com).  Hiermee opent u het blad profiel.

2. Klik op het eindpunt in de lijst.  Hiermee opent u het blad eindpunt.

3. Klik op de knop laden van het blad CDN-eindpunt.

    ![CDN-eindpunt blade](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)

    Hiermee opent u het blad laden.

    ![CDN laden blade](./media/cdn-preload-endpoint/cdn-load-blade.png)

4. Voer het volledige pad van elk actief die u wilt laden (bijvoorbeeld `/pictures/kitten.png`) in het tekstvak **pad** .

    > [AZURE.TIP] Meer **pad** tekstvakken wordt weergegeven nadat u tekst als u wilt maken van een lijst met meerdere activa invoeren.  U kunt activa in de lijst verwijderen door te klikken op de knop weglatingsteken (...).
    >
    > Paden moeten een relatieve URL die voldoet aan de volgende [reguliere expressies](https://msdn.microsoft.com/library/az24scfc.aspx): `^(?:\/[a-zA-Z0-9-_.\u0020]+)+$`.  Elk activum moet een eigen pad hebben.  Er is geen jokertekens functionaliteit voor vooraf laden activa.

    ![Knop laden](./media/cdn-preload-endpoint/cdn-load-paths.png)

5. Klik op de knop **laden** .

    ![Knop laden](./media/cdn-preload-endpoint/cdn-load-button.png)

> [AZURE.NOTE] Er is een beperking van 10 laden aanvragen per minuut per CDN profiel.

## <a name="see-also"></a>Zie ook
- [Een eindpunt Azure CDN verwijderen](cdn-purge-endpoint.md)
- [Azure CDN REST API verwijzing - leegmaakt of een vooraf geladen een eindpunt](https://msdn.microsoft.com/library/mt634451.aspx)
