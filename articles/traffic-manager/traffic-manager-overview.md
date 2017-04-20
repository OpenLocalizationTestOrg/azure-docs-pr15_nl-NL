<properties
    pageTitle="Wat is verkeer Manager | Microsoft Azure"
    description="In dit artikel vindt u begrijpen wat verkeer Manager is, en of de juiste verkeer routeren keuze voor uw toepassing informatie"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="overview-of-traffic-manager"></a>Overzicht van verkeer Manager

Microsoft Azure verkeer Manager kunt u de verdeling van de wordt gebruikersverkeer naar service-eindpunten in verschillende datacenters aangeven. Service-eindpunten worden ondersteund door verkeer Manager Azure VMs, Web Apps, opnemen en cloud services. U kunt ook verkeer Manager gebruiken met externe, niet-Azure eindpunten.

Verkeer Manager gebruikt de Domain Name System (DNS) om client verzenden naar het meest geschikte eindpunt op basis van een [methode verkeer-routing](traffic-manager-routing-methods.md) en de status van de eindpunten. Verkeer Manager biedt een bereik van verkeer-routing methoden structureren zoals u andere toepassing behoeften, de eindpunt servicestatus [bewaken](traffic-manager-monitoring.md)en automatische failover. Verkeer Manager is mislukt, inclusief de mislukken van een gehele Azure gebied tegen.

## <a name="traffic-manager-benefits"></a>Voordelen van het verkeer Manager

Verkeer Manager kan u helpen:

- **Beschikbaarheid van belangrijke toepassingen verbeteren**

    Beschikbaarheid van uw toepassingen biedt verkeer Manager door uw eindpunten cmdlets voor controle en automatische failover leveren wanneer een eindpunt uitvalt.

- **Reactiesnelheid voor krachtige toepassingen**

    Azure kunt u cloudservices of websites worden uitgevoerd in datacenters over de hele wereld. Verkeer Manager maakt de toepassing sneller door te laten lopen van verkeer naar het eindpunt met de laagste netwerklatentie voor de client.

- **Service onderhoud zonder uitvaltijd uitvoeren**

    U kunt de bewerkingen gepland onderhoud op uw toepassingen zonder uitvaltijd uitvoeren. Verkeer Manager stuurt verkeer naar alternatieve eindpunten terwijl het onderhoud uitgevoerd wordt.

- **On-premises en Cloud-toepassingen combineren**

    Verkeer Manager ondersteunt extern, niet-Azure eindpunten zodat deze kan worden gebruikt bij hybride cloud en on-premises implementaties, waaronder de "burst-naar-cloud," "migreren-naar-cloud," en 'failover cloud'-scenario's.

- **Verkeer voor grote, complexe implementaties distribueren**

    Met behulp van [geneste verkeer Manager profielen](traffic-manager-nested-profiles.md)worden verkeer-routing methoden gecombineerd om te maken van geavanceerde en flexibele regels voor het ondersteunen van de behoeften van grotere, complexe implementaties.

[AZURE.INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [de werking van verkeer Manager](traffic-manager-how-traffic-manager-works.md).

- Leer hoe u een hoge beschikbaarheid ontwikkelen met behulp van [verkeer Manager eindpunt bewaken](traffic-manager-monitoring.md).

- Meer informatie over de [omleiding van verkeer methoden](traffic-manager-routing-methods.md) worden ondersteund door verkeer Manager.

- [Een profiel verkeer Manager maken](traffic-manager-manage-profiles.md).

