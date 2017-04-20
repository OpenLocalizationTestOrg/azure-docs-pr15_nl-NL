<properties
    pageTitle="Real-permanente-stat in Azure CDN | Microsoft Azure"
    description="Realtime statistieken biedt realtime gegevens over de prestaties van Azure CDN wanneer inhoud aan uw klanten te bieden."
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

# <a name="real-time-stats-in-microsoft-azure-cdn"></a>Realtime stat in Microsoft Azure CDN

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Overzicht

In dit document wordt uitgelegd realtime stat in Microsoft Azure CDN.  Deze functionaliteit biedt realtime gegevens, zoals bandbreedte, cache statussen en verbindingen aan uw profiel CDN wanneer inhoud aan uw klanten te bieden. Hiermee worden continue monitoring van de status van uw service op elk gewenst moment, inclusief Ga live gebeurtenissen.

De volgende grafieken zijn beschikbaar:

* [Bandbreedte](#bandwidth)
* [Statuscodes](#status-codes)
* [Statussen die cache](#cache-statuses)
* [Verbindingen](#connections)


## <a name="accessing-real-time-stats"></a>Toegang krijgen tot realtime stat

1. Blader in de [Portal van Azure](https://portal.azure.com)aan uw profiel CDN.

    ![CDN profiel blade](./media/cdn-real-time-stats/cdn-profile-blade.png)

2. Klik in het blad CDN-profiel op de knop **beheren** .

    ![CDN profiel blade knop beheren](./media/cdn-real-time-stats/cdn-manage-btn.png)

    De beheerportal CDN wordt geopend.

3. Plaats de muisaanwijzer boven de tab **Analytics** en klik vervolgens muis boven de **Real-Time stat** : flyout.  Klik op **de grote Object HTTP**.

    ![CDN management portal](./media/cdn-real-time-stats/cdn-premium-portal.png)

    De realtime stat grafieken worden weergegeven.
    
Elk van de grafieken geeft realtime statistieken voor de geselecteerde tijdspanne, beginnend bij de pagina wordt geladen.  De grafieken bijwerken automatisch om de paar seconden.  De knop **Vernieuwen Graph** , wordt als deze aanwezig zijn, de grafiek, waarna deze alleen de geselecteerde gegevens weergegeven gewist.

## <a name="bandwidth"></a>Bandbreedte

![Bandbreedte graph](./media/cdn-real-time-stats/cdn-bandwidth.png)

De grafiek **bandbreedte** weergegeven hoeveel bandbreedte gebruikt voor het huidige platform via geselecteerde tijdsperiode. Het gearceerde deel van de grafiek wordt aangegeven bandbreedtegebruik. De exacte hoeveelheid bandbreedte momenteel in gebruik wordt direct onder het lijndiagram weergegeven.

## <a name="status-codes"></a>Statuscodes

![Status code graph](./media/cdn-real-time-stats/cdn-status-codes.png)

De **Status Codes** graph geeft aan hoe vaak bepaalde HTTP-antwoord codes boven de geselecteerde tijdspanne zijn optreedt.

> [AZURE.TIP]  Zie voor een beschrijving van elke optie HTTP status code, [Azure CDN HTTP-Status Codes](https://msdn.microsoft.com/library/mt759238.aspx).

Een lijst met codes voor HTTP-status wordt weergegeven boven de grafiek. Deze lijst wordt aangegeven dat elke statuscode die kan worden opgenomen in de lijngrafiek en het aantal exemplaren per seconde voor die statuscode. Een regel wordt standaard weergegeven voor elk van deze statuscodes in de grafiek. U kunt echter kiezen om alleen de de statuscodes voor waarvoor speciale significantie voor uw configuratie CDN te houden. U doet dit door de gewenste status-codes en schakelt u alle andere opties en klik op **Vernieuwen Graph**. 

U kunt logboekgegevens voor een bepaalde statuscode tijdelijk verbergen.  Klik op de legenda direct onder de grafiek op de statuscode die u wilt verbergen. De statuscode onmiddellijk verborgen uit de grafiek. Die statuscode als u opnieuw op, wordt deze optie moet opnieuw worden weergegeven.

## <a name="cache-statuses"></a>Statussen die cache

![Cache statussen graph](./media/cdn-real-time-stats/cdn-cache-status.png)

In de grafiek **Cache statussen die** wordt aangegeven hoe vaak bepaalde soorten cache statussen boven de geselecteerde tijdspanne zijn optreedt. 

> [AZURE.TIP]  Zie voor een beschrijving van elke optie cache status code, [Azure CDN Cache Status Codes](https://msdn.microsoft.com/library/mt759237.aspx).

Een lijst met codes voor cache-status wordt weergegeven boven de grafiek. Deze lijst wordt aangegeven dat elke statuscode die kan worden opgenomen in de lijngrafiek en het aantal exemplaren per seconde voor die statuscode. Een regel wordt standaard weergegeven voor elk van deze statuscodes in de grafiek. U kunt echter kiezen om alleen de de statuscodes voor waarvoor speciale significantie voor uw configuratie CDN te houden. U doet dit door de gewenste status-codes en schakelt u alle andere opties en klik op **Vernieuwen Graph**. 

U kunt logboekgegevens voor een bepaalde statuscode tijdelijk verbergen.  Klik op de legenda direct onder de grafiek op de statuscode die u wilt verbergen. De statuscode onmiddellijk verborgen uit de grafiek. Die statuscode als u opnieuw op, wordt deze optie moet opnieuw worden weergegeven.

## <a name="connections"></a>Verbindingen

![Verbindingen graph](./media/cdn-real-time-stats/cdn-connections.png)

Deze grafiek wordt aangegeven hoeveel verbindingen zijn vastgesteld om uw edge-servers. Elk verzoek om een actief dat onze CDN-resultaten in een verbinding passeert.

## <a name="next-steps"></a>Volgende stappen

- Een melding ontvangen met [realtime waarschuwingen in Azure CDN](cdn-real-time-alerts.md)
- De afdruk met [Geavanceerde HTTP-rapporten](cdn-advanced-http-reports.md)
- [Gebruikspatronen](cdn-analyze-usage-patterns.md) analyseren

