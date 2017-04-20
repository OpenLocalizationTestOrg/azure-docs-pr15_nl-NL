<properties
    pageTitle="Een eindpunt Azure CDN verwijderen | Microsoft Azure"
    description="Leer hoe u alle opgeslagen inhoud verwijderen uit een CDN-eindpunt."
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

# <a name="purge-an-azure-cdn-endpoint"></a>Een eindpunt Azure CDN verwijderen

## <a name="overview"></a>Overzicht

Azure CDN rand knooppunten cache activa totdat van het actief time to live (TTL) verloopt.  Als TTL van het actief is verlopen, wanneer een client de activa van het randknooppunt aanvraagt, het randknooppunt ophaalt een nieuwe bijgewerkte kopie van de activa moet fungeren aanvraag van de client en store de cache vernieuwen.

Soms wilt u mogelijk in de cache opgeslagen inhoud verwijderen uit alle knooppunten van de rand en voorkom dat ze alle om op te halen, nieuwe bijgewerkte activa.  Het kan zijn dat updates in uw webtoepassing, of snel update activa die onjuiste informatie bevatten.

> [AZURE.TIP] Houd er rekening mee dat alleen de inhoud in de cache opgeslagen op de CDN edge-servers verwijderen opheffen.  Een downstreamcaches, zoals proxyservers en de lokale browser cache, mogelijk nog steeds houdt u een kopie van het bestand.  Het is belangrijk om te onthouden dit als u van een bestand time to live.  U kunt een volgende client aanvragen van de nieuwste versie van het bestand door een unieke naam geven telkens wanneer u deze bijwerken of te profiteren van [caching van query-tekenreeks](cdn-query-string.md)afdwingen.  

Deze zelfstudie begeleidt u bij de activa van alle rand knooppunten van een eindpunt verwijderen.

## <a name="walkthrough"></a>Stapsgewijze instructies

1. Klik in de [Portal van Azure](https://portal.azure.com)Blader naar het CDN-profiel met het eindpunt dat u wilt verwijderen.

2. Klik op de knop opschonen van het blad CDN-profiel.

    ![CDN profiel blade](./media/cdn-purge-endpoint/cdn-profile-blade.png)

    Hiermee opent u het blad leegmaken.

    ![CDN opschonen blade](./media/cdn-purge-endpoint/cdn-purge-blade.png)

3. Selecteer het serviceadres dat u wilt verwijderen uit de vervolgkeuzelijst URL op het blad leegmaken.

    ![Afbeelding van formulier](./media/cdn-purge-endpoint/cdn-purge-form.png)

    > [AZURE.NOTE] U kunt ook het blad opschonen openen door te klikken op de knop **verwijderen** op het blad CDN-eindpunt.  In dat geval wordt het veld **URL** worden ingevuld met het serviceadres van dat specifieke eindpunt.

4. Selecteer welke activa die u wilt verwijderen uit de knooppunten rand.  Als u wissen van alle activa wilt, klikt u op het selectievakje **Alles wissen** .  Anders, typ het volledige pad van elk actief u definitief wilt verwijderen (bijvoorbeeld `/pictures/kitten.png`) in het tekstvak **pad** .

    > [AZURE.TIP] Meer **pad** tekstvakken wordt weergegeven nadat u tekst als u wilt maken van een lijst met meerdere activa invoeren.  U kunt activa in de lijst verwijderen door te klikken op de knop weglatingsteken (...).
    >
    > Paden moeten een relatieve URL die past de volgende [reguliere expressies](https://msdn.microsoft.com/library/az24scfc.aspx): `^\/(?:[a-zA-Z0-9-_.\u0020]+\/)*\*$";`.  Voor **Azure CDN uit Verizon** (standaard en Premium), sterretje (\*) mag worden gebruikt als een jokerteken (bijvoorbeeld `/music/*`).  Jokertekens en **opschonen alle** zijn niet toegestaan met **Azure CDN van Akamai**.
    
5. Klik op de knop **verwijderen** .

    ![Afbeelding van knop](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [AZURE.IMPORTANT] Leegmaken aanvragen duren ongeveer 2-3 minuten verwerken met **Azure CDN uit Verizon** (standaard en Premium) en ongeveer 7 minuten met **Azure CDN van Akamai**.  Azure CDN geldt een limiet van 50 gelijktijdige aanvragen op elk gewenst moment verwijderen. 

## <a name="see-also"></a>Zie ook
- [Vooraf geladen activa op een Azure CDN-eindpunt](cdn-preload-endpoint.md)
- [Azure CDN REST API verwijzing - leegmaakt of een vooraf geladen een eindpunt](https://msdn.microsoft.com/library/mt634451.aspx)
