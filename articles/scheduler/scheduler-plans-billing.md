<properties
 pageTitle="Abonnementen en facturering in Azure Scheduler"
 description="Abonnementen en facturering in Azure Scheduler"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="plans-and-billing-in-azure-scheduler"></a>Abonnementen en facturering in Azure Scheduler

## <a name="job-collection-plans"></a>Taak siteverzamelingen plannen

Taak verzamelingen zijn de factureerbare entiteit in Azure Scheduler. Taak verzamelingen bevatten een aantal taken en komt in drie pakketten – gratis, Standard en Premium – die hieronder worden beschreven.

|**Plan voor het verzamelen van taak**|**Maximale aantal taken per taak siteverzameling**|**Max terugkeerpatroon**|**De functie Max siteverzamelingen per abonnement**|**Limieten**|
|:---|:---|:---|:---|:---|
|**Vrij te geven**|5 taken per taak siteverzameling|Één keer per uur. Taken vaker dan één keer per uur kan niet worden uitgevoerd|Een abonnement is maximaal 1 gratis taak siteverzameling toegestaan|[HTTP uitgaande autorisatie object](scheduler-outbound-authentication.md) niet gebruiken
|**Standaard**|50 taken per taak siteverzameling|Eenmaal per minuut. Kan vaker dan eens per minuut taken niet uitvoeren|Een abonnement is maximaal 100 standaard taak verzamelingen toegestaan|Toegang tot de volledige functieset van Scheduler|
|**P10 Premium**|50 taken per taak siteverzameling|Eenmaal per minuut. Kan vaker dan eens per minuut taken niet uitvoeren|Een abonnement mag maximaal 10.000 P10 Premium taak siteverzamelingen. <a href="mailto:wapteams@microsoft.com">Contact met ons opnemen</a> voor meer informatie.|Toegang tot de volledige functieset van Scheduler|
|**P20 Premium**|1000-taken per taak siteverzameling|Eenmaal per minuut. Kan vaker dan eens per minuut taken niet uitvoeren|Een abonnement mag maximaal 10.000 P20 Premium taak siteverzamelingen. <a href="mailto:wapteams@microsoft.com">Contact met ons opnemen</a> voor meer informatie.|Toegang tot de volledige functieset van Scheduler|

## <a name="upgrades-and-downgrades-of-job-collection-plans"></a>Upgrades en Downgrades van taak siteverzamelingen plannen

U kunt upgraden of een taak siteverzameling plan op elk gewenst moment over de abonnementen gratis, Standard en Premium downgraden. Echter wanneer een downgrade uitvoeren aan een verzameling gratis taak, mislukken het downgrade voor een van de volgende oorzaken hebben:

- Een verzameling gratis taak al bestaat in het abonnement
- Een taak in de siteverzameling van de taak heeft een hogere terugkeerpatroon dan toegestaan voor projecten in gratis taak siteverzamelingen. Het maximum aantal terugkeerpatroon toegestaan in een verzameling gratis taak is één keer per uur
- Er zijn meer dan 5 taken in de verzameling taak
- Een taak in de siteverzameling van de taak heeft een HTTP of HTTPS actie die gebruikmaakt van een [HTTP uitgaande autorisatie object](scheduler-outbound-authentication.md)

## <a name="billing-and-azure-plans"></a>Facturering en Azure-abonnementen

Abonnementen zijn geen btw geheven gratis verzamelingen van de taak. Als u meer dan 100 standaard taak verzamelingen (10 standaard billing eenheden) hebt, is een betere koop om alle collecties van de taak in de premium-abonnement.

Als u één standaard taak siteverzameling en één premium taak siteverzameling hebt, bent u gefactureerde één factuur per eenheid _en_ één premium billing standaardeenheid. De Scheduler service facturen op basis van het aantal actieve taak siteverzamelingen die zijn ingesteld op standard of premium; Dit wordt uitgelegd verder in de volgende twee secties.

## <a name="standard-billable-units"></a>Standaard factureerbare eenheden

Een factureerbare standaardeenheid kan maximaal 10 standaard taak verzamelingen bevatten. Aangezien een verzameling standaard taak maximaal 50 taken per taak siteverzameling hebben kan, kan één billing standaardeenheid een abonnement heeft maximaal 500 taken – snel aan de uitvoering bijna 22 miljoen per maand.

Als u tussen 1 en 10 standaard taak verzamelingen hebt, worden u voor 1 billing standaardeenheid gefactureerd. Als u tussen 11 en 20 standaard taak verzamelingen hebt, worden u voor 2 standaard billing eenheden gefactureerd. Als u tussen 21 en 30 standaard taak verzamelingen, u voor 3 standaard billing eenheden worden gefactureerd, enzovoort.

## <a name="p10-premium-billable-units"></a>P10 Premium factureerbare eenheden

Een P10 premium factureerbare eenheid kan maximaal 10.000 P10 premium taak verzamelingen bevatten. Aangezien een verzameling P10 premium taak maximaal 50 taken per taak siteverzameling hebt kan, kunt u één premium billing eenheid een abonnement op maximaal 500.000 taken – snel aan de uitvoering per maand bijna 22 miljard hebt.

Als u tussen 1 en 10.000 premium taak verzamelingen hebt, worden u voor 1 P10 premium billing eenheid gefactureerd. Als u tussen 10,001 en 20.000 premium taak verzamelingen, u voor 2 P10 premium billing eenheden worden gefactureerd, enzovoort.

Dus P10 premium taak verzamelingen hebben dezelfde functionaliteit als de standaard taak verzamelingen maar u kunt zelf een einde prijs geval uw toepassing een groot aantal verzamelingen van de taak vereist.

## <a name="p20-premium-billable-units"></a>P20 Premium factureerbare eenheden

Een P20 premium factureerbare eenheid kan maximaal 5.000 P20 premium taak verzamelingen bevatten. Aangezien een verzameling P20 premium taak maximaal 1000 taken per taak siteverzameling hebt kan, kunt u één premium billing eenheid een abonnement op maximaal 5,000,000 taken – snel aan de uitvoering per maand bijna 220 miljard hebt.

P20 premium taak verzamelingen beschikt u over dezelfde voorzieningen als P10 premium taak verzamelingen, maar ook een groter getal taken per taak siteverzameling en een groter totaal aantal taken algehele dan P10 premium waarmee u meer schaalbaarheid ondersteunt.

## <a name="billing-and-active-status"></a>Facturerings- en actieve Status

Taak verzamelingen zijn altijd actief, tenzij uw hele abonnement enkele tijdelijke uitgeschakeld geworden vanwege problemen facturering. De enige manier om ervoor te zorgen dat de siteverzameling van een taak niet wordt gefactureerd is naar een instellen het _gratis_ abonnement of de verzameling taak verwijderen.

Hoewel u alle taken binnen de verzameling van een taak in één bewerking kunt uitschakelen, de facturering status van de siteverzameling van de taak niet verandert – de verzameling taak wordt worden _nog steeds_ gefactureerd. Daarnaast lege taak verzamelingen worden beschouwd als actief en wordt gefactureerd.

## <a name="pricing"></a>Prijzen

Zie [Scheduler prijzen](https://azure.microsoft.com/pricing/details/scheduler/)voor prijzen details.

## <a name="see-also"></a>Zie ook


 [Wat is Scheduler?](scheduler-intro.md)

 [Azure Scheduler concepten, terminologie en entiteitenhiërarchie](scheduler-concepts-terms.md)

 [Aan de slag met Scheduler in de portal van Azure](scheduler-get-started-portal.md)

 [Azure Scheduler REST API-verwijzing](https://msdn.microsoft.com/library/mt629143)

 [Overzicht van Azure Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Azure Scheduler hoge beschikbaarheid en betrouwbaarheid](scheduler-high-availability-reliability.md)

 [Azure Scheduler limieten, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)

 [Azure Scheduler uitgaande verificatie](scheduler-outbound-authentication.md)
