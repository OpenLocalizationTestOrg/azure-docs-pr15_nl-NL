<properties 
   pageTitle="Gegevensbronnen in Log Analytics | Microsoft Azure"
   description="Gegevensbronnen definiëren de dat Log Analytics verzameld uit agenten en andere bronnen verbonden gegevens.  In dit artikel wordt het concept van hoe Log Analytics gebruik van gegevensbronnen, wordt de details van het configureren van deze uitgelegd en bevat een overzicht van de verschillende gegevensbronnen beschikbaar."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="data-sources-in-log-analytics"></a>Gegevensbronnen in Log Analytics

Log analyses verzamelt gegevens uit de bronnen verbonden in uw werkruimte OMS en opgeslagen in de bibliotheek OMS.  De gegevens die worden verzameld uit elk is gedefinieerd door de gegevensbronnen die u wilt configureren.  Gegevens in de bibliotheek OMS wordt opgeslagen als een set records.  Elke gegevensbron maakt records van een bepaald type met elk type met een eigen set met eigenschappen.

![Meld u aan analyses gegevens verzamelen](./media/log-analytics-data-sources/overview.png)

Gegevensbronnen zijn anders dan het OMS-oplossingen die ook gegevens verzamelen van bronnen verbonden en records in de bibliotheek OMS maken.  Oplossingen kunnen worden toegevoegd aan uw werkruimte vanuit de galerie met oplossingen en, meestal vindt u aanvullende analysehulpmiddelen in de portal OMS.  

## <a name="summary-of-data-sources"></a>Overzicht van gegevensbronnen

De gegevensbronnen die momenteel beschikbaar in Log Analytics zijn worden weergegeven in de volgende tabel.  Elk heeft een koppeling naar een afzonderlijk artikel detail leveren voor de gegevensbron.

| Gegevensbron | Gebeurtenistype | Beschrijving |
|:--|:--|:--|
| [Aangepaste logboeken](log-analytics-data-sources-custom-logs.md) | \<Logboeknaam\>_CL | Tekstbestanden op Windows of Linux agenten met logboekgegevens. |
| [Gebeurtenislogboeken van Windows](log-analytics-data-sources-windows-events.md) | Gebeurtenis | Gebeurtenissen die worden verzameld uit het gebeurtenislogboek op een Windows-computer. |
| [Windows prestatie-items](log-analytics-data-sources-performance-counters.md) | Perf | Van prestatiemeteritems die worden verzameld van Windows-computers. |
| [Linux prestatie-items](log-analytics-data-sources-performance-counters.md) | Perf | Van prestatiemeteritems die worden verzameld van Linux computers. |
| [IIS-logboeken](log-analytics-data-sources-iis-logs.md) | W3CIISLog | Internet Information Services Logboeken in W3C-indeling. |
| [Syslog](log-analytics-data-sources-syslog.md) | Syslog | Syslog gebeurtenissen op Windows- of Linux-computer. |

## <a name="configuring-data-sources"></a>Gegevensbronnen configureren

U configureert gegevensbronnen in het menu **Data** in Log Analytics- **Instellingen**.  De configuratie is afgeleverd in alle gekoppelde gegevensbronnen in uw werkruimte OMS.  U kunt geen eventuele agenten momenteel uitsluiten van deze configuratie.

![Configureren welke gebeurtenissen Windows](./media/log-analytics-data-sources/configure-events.png)

2. Selecteer de tegel **-Instellingen** in de OMS-console.
3. Selecteer **gegevens**.
4. Klik op de gegevensbron te configureren.
5. Volg de koppeling naar de documentatie voor elke gegevensbron in de bovenstaande tabel voor meer informatie op hun configuratie.

## <a name="data-collection"></a>Gegevens verzamelen

Gegevensbron configuraties zijn afgeleverd in agenten die rechtstreeks met OMS binnen enkele minuten verbonden zijn.  De opgegeven gegevens is verzameld uit de agent en geabonneerd rechtstreeks naar Log analyses met tussenpozen die specifiek zijn voor elke gegevensbron.  Zie de documentatie voor elke gegevensbron voor deze specifieke informatie over.

Als u wilt toevoegen in een groep verbonden management System Center Operations Manager (SCOM), zijn gegevensbron configuraties management packs vertaald en afgeleverd in de groep management elke 5 minuten al dan niet standaard.  De agent het management pack zoals elke andere downloads en de opgegeven gegevens verzameld. Afhankelijk van de gegevensbron de gegevens worden dat hetzij verzonden naar een management server die de gegevens naar het logboek Analytics doorstuurt of de agent stuurt de gegevens naar Log Analytics zonder tussenkomst van de server. Raadpleeg [siteverzameling detailgegevens voor OMS functies en oplossingen](log-analytics-add-solutions.md#data-collection-details-for-oms-features-and-solutions) voor meer informatie.  U kunt lezen over de details van het verbinden van SCOM en OMS en wijzigen van de frequentie die configuratie op [Configureren integratie met System Center Operations Manager](log-analytics-om-agents.md)wordt afgeleverd.

## <a name="log-analytics-records"></a>Logboekrecords Analytics

Alle gegevens die worden verzameld door Log Analytics wordt opgeslagen in de bibliotheek OMS als records.  Records die zijn verzameld met verschillende gegevensbronnen heeft een eigen set met eigenschappen en worden aangeduid door de eigenschap **Type** .  Zie de documentatie voor elke gegevensbron en oplossing voor meer informatie over elk recordtype.


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [oplossingen](log-analytics-add-solutions.md) die functionaliteit toevoegen aan Log Analytics en gegevens ook naar de bibliotheek OMS verzamelen.
- Meer informatie over [log zoekopdrachten](log-analytics-log-searches.md) om de gegevens die worden verzameld uit gegevensbronnen en oplossingen te analyseren.  
- [Waarschuwingen](log-analytics-alerts.md) proactief melding krijgt van belangrijke gegevens die worden verzameld uit gegevensbronnen en oplossingen configureren.
