<properties 
   pageTitle="Prestaties van Windows en Linux items in Log Analytics | Microsoft Azure"
   description="Prestatie-items worden verzameld door Log Analytics te analyseren prestaties van Windows en Linux agenten.  In dit artikel wordt beschreven hoe u verzameling prestatiemeteritems voor zowel Windows en Linux agenten, details van deze zijn opgeslagen in de bibliotheek OMS en hoe u ze in de portal OMS analyseren."
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
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="windows-and-linux-performance-data-sources-in-log-analytics"></a>Windows en Linux prestaties gegevensbronnen in Log Analytics 

Van prestatiemeteritems in Windows en Linux bieden inzicht in de prestaties van hardwareonderdelen, besturingssystemen en toepassingen.  Log Analytics kan prestatiemeteritems verzamelen met regelmatige tussenpozen voor analyse in de buurt realtime (NRT) naast het aggregeren prestatiegegevens voor een langere periode analyse en rapportage.

![Prestatie-items](media/log-analytics-data-sources-performance-counters/overview.png)

## <a name="configuring-performance-counters"></a>Prestatie-items configureren

Prestaties van items in het [menu van de gegevens in Log Analytics-instellingen](log-analytics-data-sources.md#configuring-data-sources)configureren.

Als u Windows of Linux-prestaties items voor een nieuwe OMS-werkruimte voor het eerst configureert, krijgt u de optie voor diverse algemene items snel te maken.  Deze worden weergegeven met een selectievakje in naast elk.  Zorg ervoor dat alle items die u wilt maken in eerste instantie zijn ingeschakeld en klik vervolgens op **de geselecteerde items toevoegen**.

![Windows prestatie-items configureren](media/log-analytics-data-sources-performance-counters/configure-windows.png)

Volg deze procedure om een nieuw item van de prestaties van Windows voor het verzamelen van toevoegen.

1. Typ de naam van het item in het tekstvak in de indeling *\counter object (exemplaar)*.  Wanneer u te typen begint, verschijnt er een overeenkomende lijst met algemene items.  U kunt een item uit de lijst of typ in een eigen selecteren.  U kunt ook alle exemplaren van een bepaald item terugkeren door het opgeven van *object item*. 
2. Klik op **+** of druk op **Enter** om het item toevoegen aan de lijst.
3. Wanneer u een item toevoegt, wordt de standaardwaarde van 10 seconden voor de **Controle-Interval**gebruikt.  U kunt dit wijzigen naar een hogere waarde van maximaal 1800 seconden (30 minuten) als u wilt de opslagvereisten van de verzamelde prestatiegegevens verkleinen.
4. Wanneer u klaar bent met het toe te voegen items, klikt u op de knop **Opslaan** boven aan het scherm de configuratie opslaan.

![Linux prestatie-items configureren](media/log-analytics-data-sources-performance-counters/configure-linux.png)

Volg deze procedure om een nieuw item van de prestaties Linux voor het verzamelen van toevoegen.

1. Standaard worden alle configuratiewijzigingen worden automatisch verplaatst naar alle agenten.  Als u Linux wilt toevoegen, wordt een configuratiebestand verzonden naar de Fluentd gegevens verzamelen.  Als u wijzigen van dit bestand handmatig op elk Linux-agent wilt, schakelt u het vak *toepassen onder configuratie voor mijn computers Linux*.
2. Typ de naam van het item in het tekstvak in de indeling *\counter object (exemplaar)*.  Wanneer u te typen begint, verschijnt er een overeenkomende lijst met algemene items.  U kunt een item uit de lijst of typ in een eigen selecteren.  
2. Klik op **+** of druk op **Enter** om het item toevoegen aan de lijst met items van het object.
3. Alle items voor een object gebruik hetzelfde **Controle-Interval**.  De standaardinstelling is 10 seconden.  U Wijzig deze in een hogere waarde van maximaal 1800 seconden (30 minuten) als u wilt de opslagvereisten van de verzamelde prestatiegegevens verkleinen.
4. Wanneer u klaar bent met het toe te voegen items, klikt u op de knop **Opslaan** boven aan het scherm de configuratie opslaan.

## <a name="data-collection"></a>Gegevens verzamelen

Log Analytics verzamelt alle opgegeven prestatiemeteritems op hun opgegeven controle-interval op alle agenten die die teller geïnstalleerd hebt.  De gegevens niet worden samengevoegd en de onbewerkte gegevens is beschikbaar in alle log zoeken weergaven voor de duur die is opgegeven door uw OMS-abonnement.


## <a name="performance-record-properties"></a>Prestaties recordeigenschappen

Prestaties records een soort **Perf** en hebt de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| Computer         | Computer waarop de gebeurtenis is verzameld uit. |
| CounterName      | Naam van het prestatiemeteritem |
| Itempad      | Volledige pad van het item in het formulier \\ \\ \<Computer >\\object(exemplaar)\\item. |
| Tegenwaarde     | Numerieke waarde van het item.  |
| Exemplaarnaam     | Naam van het exemplaar van de gebeurtenis.  Als er geen exemplaar leegmaken. |
| Objectnaam       | Naam van het prestatieobject |
| SourceSystem  | Type agent die de gegevens zijn verzameld uit. <br> OpsManager – Windows-agent, een van beide direct verbinding maken of SCOM <br> Linux – alle Linux agenten  <br> AzureStorage – Azure diagnostische gegevens |
| TimeGenerated       | Datum en tijd is dat met de gegevens. |


## <a name="sizing-estimates"></a>Maakt een schatting van formaatgrepen

 Een ruw schatting voor de siteverzameling van een bepaald item met 10 seconden tussenpozen is ongeveer 1 MB per dag per exemplaar.  U kunt de opslagvereisten van een bepaald item met de volgende formule schatten.

    1 MB x (number of counters) x (number of agents) x (number of instances)

## <a name="log-searches-with-performance-records"></a>Zoekopdrachten in het logboek met prestaties records

De volgende tabel bevat verschillende voorbeelden van log zoekopdrachten die prestaties records ophalen.

| Query | Beschrijving |
|:--|:--|
| Typ = Perf | Alle prestatiegegevens |
| Typ = Perf Computer = "Computer" | Alle gegevens van de prestaties van een bepaalde computer |
| Typ = Perf CounterName = "Huidige schijf wachtrijlengte" | Alle prestatiegegevens voor een bepaald item |
| Typ = Perf (objectnaam = Processor) CounterName = "% Processor Time" exemplaarnaam = _Totaal & #124; eenheid Avg(Average) als AVGCPU door Computer | Gemiddelde CPU-gebruik op alle computers |
| Typ = Perf (CounterName = "% Processor Time") & #124;  max(Max) door Computer meten | Maximale CPU-gebruik op alle computers |
| Type = Perf objectnaam = logische CounterName = Computer "Huidige schijf wachtrijlengte" = "MijnComputernaam" & #124; Avg(Average) door exemplaarnaam meten | Gemiddelde van de huidige schijfwachtrij lengte in alle exemplaren van een bepaalde computer |
| Typ = Perf CounterName = "DiskTransfers/sec" & #124; percentile95(Average) door Computer meten | 95e percentiel van schijf overdrachten/Sec op alle computers |
| Typ = Perf CounterName = "% Processor Time" exemplaarnaam = "_Totaal" & #124; avg(CounterValue) meten door Computer Interval 1 uur | Ieder uur gemiddelde van CPU-gebruik op alle computers |
| Typ = Perf Computer = "Computer" CounterName = % * exemplaarnaam = _Totaal & #124; eenheid percentile70(CounterValue) door CounterName Interval 1 uur | Ieder uur 70 percentiel van elke percentage item percentage voor een bepaalde computer |
| Type Perf CounterName = "% Processor Time" exemplaarnaam = "_Totaal" = (Computer = "Computer") & #124; min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) meten door Computer Interval 1 uur | Ieder uur gemiddelde, minimum, maximum en 75 percentiel CPU-gebruik voor een specifieke computer |

## <a name="viewing-performance-data"></a>Prestatiegegevens weergeven

Wanneer u een zoekopdracht log voor prestatiegegevens uitvoert, wordt de weergave **logboek** al dan niet standaard weergegeven.  Als u wilt de gegevens in een grafische formulier bekijkt, klikt u op **de doelstellingen**.  Voor een gedetailleerde grafische weergave, klikt u op de **+** naast een item.  

![Aan de doelstellingen weergave samengevouwen](media/log-analytics-data-sources-performance-counters/metricscollapsed.png)

Als het tijdsbereik die u hebt geselecteerd 6 uur of kleiner is, wordt de grafiek om de paar seconden bijgewerkt.  De actuele gegevens wordt weergegeven aan de rechterkant van de grafiek op lichtblauw.

![Aan de doelstellingen weergave uitgebreid met live gegevens](media/log-analytics-data-sources-performance-counters/metricsexpanded.png)

Prestatiegegevens in een zoekopdracht log aggregeren: Zie Help [op aanvraag metrische aggregatie- en visualisatiefilters in OMS](http://blogs.technet.microsoft.com/msoms/2016/02/26/on-demand-metric-aggregation-and-visualization-in-oms/).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [log zoekopdrachten](log-analytics-log-searches.md) om de gegevens die worden verzameld uit gegevensbronnen en oplossingen te analyseren.  
- Verzamelde gegevens exporteren naar [Power BI](log-analytics-powerbi.md) voor extra visualisaties en analyse.