<properties
    pageTitle="Azure CDN caching van gedrag van aanvragen met querytekenreeksen bepalen | Microsoft Azure"
    description="Azure CDN-queryreeks caching van besturingselementen hoe bestanden worden wanneer deze querytekenreeksen bevatten in de cache opgeslagen."
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

#<a name="controlling-caching-behavior-of-cdn-requests-with-query-strings"></a>Beheren in de cache gedrag van CDN aanvragen met querytekenreeksen met de

> [AZURE.SELECTOR]
- [Standaard](cdn-query-string.md)
- [Azure CDN Premium uit Verizon](cdn-query-string-premium.md)

##<a name="overview"></a>Overzicht

Queryreeks caching van besturingselementen hoe bestanden worden wanneer deze querytekenreeksen bevatten in de cache opgeslagen.

> [AZURE.IMPORTANT] De standaardweergave en de Premium-CDN-producten bieden dezelfde querytekenreeks cache-functionaliteit, maar verschilt van de gebruikersinterface.  In dit document beschreven hoe de interface voor **Azure CDN standaard uit Akamai** - en **Azure CDN van Verizon**.  Zie voor query tekenreeks caching met **Azure CDN Premium uit Verizon**, [Controlling in de cache gedrag van CDN aanvragen met querytekenreeksen met de - Premium](cdn-query-string-premium.md).

Er zijn drie modi beschikbaar:

- **Negeren querytekenreeksen**: dit is de standaardmodus.  Het knooppunt van de rand CDN wordt de queryreeks doorgeven van degene naar de oorsprong op de eerste aanvraag en de cache de activa.  Alle volgende aanvragen voor de activa die vanaf het randknooppunt bereikbaar zijn negeert de queryreeks totdat de in de cache activa verloopt.
- **Overslaan URL met querytekenreeksen met de in cache opslaan**: In deze modus aanvragen met querytekenreeksen met de worden niet in cache bij het knooppunt van de rand CDN.  Het randknooppunt opgehaald van de activa rechtstreeks vanuit de oorsprong en wordt doorgegeven aan degene met elk verzoek om.
- **Elke unieke URL in de cache**: deze modus wordt elk verzoek om met een queryreeks verwerkt als een unieke actief met een eigen cache.  Bijvoorbeeld geval de reactie van de oorsprong voor een verzoek om *foo.ashx?q=bar* in cache bij het randknooppunt en geretourneerd voor latere cache met die dezelfde querytekenreeks.  Een aanvraag voor *foo.ashx?q=somethingelse* zou doen in de cache als een afzonderlijk actief met een eigen tijd naar live.

##<a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Wijzigen van de queryreeks caching van instellingen voor standaard CDN-profielen

1. Klik op het blad CDN-profiel op het CDN-eindpunt dat u wilt beheren.

    ![CDN profiel blade eindpunten](./media/cdn-query-string/cdn-endpoints.png)

    Hiermee opent u het blad CDN-eindpunt.

2. Klik op de knop **configureren** .

    ![CDN profiel blade knop beheren](./media/cdn-query-string/cdn-config-btn.png)

    Hiermee opent u het blad CDN configuratie.

3. Selecteer een instelling in de vervolgkeuzelijst **queryreeks caching van gedrag** .

    ![CDN-queryreeks cache-opties](./media/cdn-query-string/cdn-query-string.png)

4. Nadat u uw keuze hebt, klikt u op de knop **Opslaan** .

> [AZURE.IMPORTANT] De instellingen wijzigingen zijn mogelijk niet direct zichtbaar zijn, als het duurt voor de registratie tot en met de CDN doorgeven.  Voor <b>Azure CDN van Akamai</b> profielen wordt meestal doorgeven voltooid binnen één minuut.  Voor <b>Azure CDN uit Verizon</b> profielen doorgeven duurt meestal binnen 90 minuten, maar in sommige gevallen kan langer duren.
