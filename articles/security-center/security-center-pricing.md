<properties
   pageTitle="Beveiligingscentrum prijzen | Microsoft Azure"
   description="In dit artikel wordt aandacht besteed aan prijzen voor Beveiligingscentrum Azure."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/12/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-pricing"></a>Azure Beveiligingscentrum prijzen

Azure Beveiligingscentrum kunt u voorkomen dat, detecteren en reageren op threats met deze beter kunt zien in en controle over de beveiliging van uw Azure resources. Deze biedt geïntegreerde beveiliging cmdlets voor controle en beleid verschillende uw Azure-abonnementen, helpt detecteren threats die mogelijk anders mis en werkt met een brede selectie van beveiligingsoplossingen.

## <a name="pricing-tiers"></a>Prijzen van lagen

Beveiligingscentrum wordt aangeboden in twee lagen:

- De **gratis laag** is automatisch ingeschakeld op alle Azure abonnementen. De gratis laag biedt inzicht in de beveiligingsstatus van uw Azure resources, eenvoudige beveiligingsbeleid, aanbevelingen voor beveiliging en integratie met beveiligingsproducten en services van partners.
- De **standaard laag** voegt geavanceerde bedreiging detectie-functies, inclusief bedreiging bedrijfsinformatie, doorgevoerd analyse, afwijking detectie, -incidenten en bedreiging assessment rapporten. Een **gratis proefversie van 90 dagen** is beschikbaar voor de standaard laag.

Zie de Beveiligingscentrum [prijzen van de pagina](https://azure.microsoft.com/pricing/details/security-center/)voor meer informatie.

> [AZURE.NOTE] Beveiligingscentrum gebruikt Azure opslag om op te slaan beveiligingsgegevens die zijn gegenereerd op basis van de beveiligde knooppunten. Kosten die is gekoppeld aan deze opslag worden niet opgenomen in de prijs van de service en afzonderlijk wordt geheven op normale [Azure opslag tarieven](https://azure.microsoft.com/pricing/details/storage/blobs/). Opslag gesprekskosten zijn van toepassing ook tijdens het proefabonnement.

## <a name="try-standard-free-for-90-days"></a>Probeer standaard gratis uit gedurende 90 dagen

Een gratis proefversie van 90 dagen is beschikbaar voor de standaard laag. Als u de gratis proefversie van de standaard laag, selecteert u de tegel **beleid** op het blad **Beveiligingscentrum** . Selecteer het abonnement dat u wilt upgraden naar standaard. Selecteer op het blad **beveiligingsbeleid** **prijzen laag**. Selecteer op het blad **kiezen uw prijzen laag** **standaard – gratis proefversie**.

![Gratis proefversie][1]

Aan het einde van 90 dagen, moet u kiezen om door te gaan met de service, wordt automatisch gestart opladen voor gebruik.

## <a name="upgrade-to-standard"></a>Een upgrade uitvoeren naar standaard

Upgrade uitvoeren naar de standaard laag geavanceerde bedreiging detectie toevoegen. Als u de standaard laag, selecteert u de tegel **beleid** op het blad **Beveiligingscentrum** . Selecteer het abonnement dat u wilt upgraden naar standaard. Selecteer op het blad **beveiligingsbeleid** **prijzen laag**. Selecteer op het blad **kiezen uw prijzen laag** de optie **standaard**.

![Standaard laag][2]

## <a name="why-upgrade-to-standard"></a>Waarom upgraden naar standaard?

De standaard laag van Beveiligingscentrum vindt u alle functies van de gratis laag plus geavanceerde bedreiging detectie. Geavanceerde bedreiging detectie helpt actieve threats doelgroepen uw Azure resources identificeren en biedt u de inzichten die nodig zijn voor het snel reageren.

Beveiligingscentrum maakt gebruik van geavanceerde beveiligingsinstellingen analytics, die verder gaan uiterst dan methoden op basis van een handtekening. Doorbraken in big data en machine learning-technologieën wordt gebruikt als u wilt evalueren gebeurtenissen over de hele cloud stof – detecteren threats die is niet mogelijk om handmatig benaderingen en voorspellingen te doen de ontwikkeling van aanvallen te bepalen.

Beveiliging analytics die worden meegeleverd bij de standaard laag zijn:

- **Bedreiging intelligence** - Hiermee wordt gezocht naar bekende bad betrokkenen met behulp van de globale bedreiging bedrijfsinformatie van Microsoft-producten en services, de eenheid voor digitale misdrijven van Microsoft, de Microsoft Security Response Center en externe-feeds
- **Doorgevoerd analyse** - is van toepassing bekende patronen om te ontdekken schadelijke gedrag
- **Afwijking detectie** - gebruikt statistische profielen voor het maken van een historische basislijn. Deze meldingen over afwijkingen van waarop deze is opgezet basislijnen die aan een mogelijke aanvalsvector voldoen

Klik in het onderstaande blad **beveiligingsmeldingen** heeft Beveiligingscentrum **incident**gedetecteerd. Een beveiligingsincident is een samenvoeging van alle waarschuwingen voor een resource die met kill ketting patronen overeenkomen. De beveiligingsincident selecteren klikt, worden meer informatie over het incident en de meldingen voor gerelateerde lijsten. Een waarschuwing selecteren, vindt u meer informatie over dat exemplaar.

![Beveiligingsincident][3]

De onderstaande **netwerkcommunicatie** -melding bevat informatie over de waarschuwing. Details zijn de volledige beschrijving, de ernst, de huidige status (die in dit geval wordt geannuleerd, wat betekent dat de gebruiker heeft actie om deze te verwijderen), het aangevallen resource en remediation stappen. Er is ook een lijst met koppelingen naar Microsoft bedreiging Intelligence-rapporten. Deze rapporten kunnen worden gebruikt voor beveiliging remediation en defensief doeleinden.

![Waarschuwing beveiligingsinformatie][4]

## <a name="enable-data-collection"></a>Gegevens verzamelen inschakelen

Als u wilt inschakelen VM doorgevoerd analyses, moet verzamelen van gegevens worden ingeschakeld. U moet verzamelen inschakelen wanneer u eerst Beveiligingscentrum openen of wanneer u een beveiligingsbeleid maakt.

Om te valideren dat verzamelen van gegevens is ingeschakeld, selecteert u de tegel **beleid** . Het blad **beveiligingsbeleid** verschijnt lijst van uw Azure-abonnementen. Selecteer een abonnement. Als **gegevens verzamelen** uitgeschakeld is, verandert u deze in op en sla de wijziging. Zie [gegevens verzamelen in Azure Beveiligingscentrum inschakelen](security-center-enable-data-collection.md).

## <a name="next-steps"></a>Volgende stappen

- In dit document, zijn u kennis met prijzen voor Beveiligingscentrum. Zie de Beveiligingscentrum [pagina prijzen](https://azure.microsoft.com/pricing/details/security-center/)voor aanvullende prijsgegevens.
- Zie meer informatie over de mogelijkheden van de detectie van geavanceerde Beveiligingscentrum van [Azure Beveiligingscentrum detectiemogelijkheden](security-center-detection-capabilities.md).
- Als u vragen over het gebruik van Beveiligingscentrum hebt, raadpleegt u de [Veelgestelde vragen over Azure beveiliging beheercentrum](security-center-faq.md).
- Als u nog steeds vragen over het gebruik van Beveiligingscentrum of Azure hebt, bezoekt u de [Azure-Forums](https://social.msdn.microsoft.com/Forums/home?forum=AzureSecurityCenter&filter=alltypes&sort=lastpostdesc).

<!--Image references-->
[1]: ./media/security-center-pricing/free-trial.png
[2]: ./media/security-center-pricing/standard.png
[3]: ./media/security-center-pricing/incident.png
[4]: ./media/security-center-pricing/network-alert.png
