<properties 
   pageTitle="Windows-gebeurtenislogboeken in Log Analytics | Microsoft Azure"
   description="Gebeurtenislogboeken van Windows zijn een van de meest voorkomende gegevensbronnen die worden gebruikt door Log Analytics.  In dit artikel wordt beschreven hoe verzameling gebeurtenislogboeken van Windows en details van de records die ze in de bibliotheek OMS maken configureren."
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

# <a name="windows-event-log-data-sources-in-log-analytics"></a>Windows-logboek gegevensbronnen in Log Analytics

Gebeurtenislogboeken van Windows zijn een van de meest voorkomende [gegevensbronnen](log-analytics-data-sources.md) gebruikt voor Windows kunt vinden, aangezien dit de methode die wordt gebruikt door de meeste toepassingen aan te melden informatie en fouten.  U kunt gebeurtenissen van standaard logboeken zoals systeem- en verzamelen naast het opgeven van alle aangepaste logboeken gemaakt door de toepassingen die u nodig hebt om te controleren.

![Windows-gebeurtenissen](media/log-analytics-data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Configureren Windows gebeurtenislogboeken

Gebeurtenislogboeken van Windows in het [menu van de gegevens in Log Analytics-instellingen](log-analytics-data-sources.md#configuring-data-sources)configureren.

Log Analytics worden alleen gebeurtenissen verzameld uit de gebeurtenislogboeken van Windows die zijn opgegeven in de instellingen.  U kunt een nieuw logboek toevoegen door te typen in de naam van het logboek en te klikken op **+**.  Voor elk logboek worden alleen gebeurtenissen met de geselecteerde severities verzameld.  Controleer de severities voor het bepaalde logboek die u wilt verzamelen.  U kunt geen eventuele aanvullende criteria te Filtergebeurtenissen opgeeft.

![Configureren welke gebeurtenissen Windows](media/log-analytics-data-sources-windows-events/configure.png)


## <a name="data-collection"></a>Gegevens verzamelen

Log Analytics verzamelt elke gebeurtenis die overeenkomt met een geselecteerde ernst van een gecontroleerde gebeurtenislogboek tijdens de gebeurtenis is gemaakt.  De agent record, wordt de positie in elke gebeurtenislogboek die worden verzameld uit.  Als de agent voor een periode offline gaat, klikt u vervolgens verzamelen Log Analytics gebeurtenissen uit waar deze laatste gebleven was, zelfs als de gebeurtenissen die zijn gemaakt terwijl de-agent offline is.


## <a name="windows-event-records-properties"></a>Eigenschappen van Windows gebeurtenis records

Windows gebeurtenisrecords hebt een type **gebeurtenis** en hebt de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| Computer            | Naam van de computer waarop de gebeurtenis is verzameld uit. |
| Culture       | De categorie van de gebeurtenis. |
| EventData           | Alle gebeurtenisgegevens in de onbewerkte indeling. |
| EventID             | Nummer van de gebeurtenis. |
| EventLevel          | Ernst van de gebeurtenis in numerieke formulier. |
| EventLevelName      | Ernst van de gebeurtenis in tekstvorm. |
| Gebeurtenislogboek            | De naam van het gebeurtenislogboek die de gebeurtenis is verzameld uit. |
| ParameterXml        | Gebeurtenis-parameterwaarden in XML-indeling. |
| ManagementGroupName | De naam van de management group voor SCOM agenten.  Voor andere gemachtigden is dit de AOI-<workspace ID> |
| RenderedDescription | De beschrijving van de gebeurtenis met parameterwaarden |
| Bron              | Bron van de gebeurtenis. |
| SourceSystem  | Type agent de gebeurtenis zijn verzameld uit. <br> OpsManager – Windows-agent, een van beide direct verbinding maken of SCOM <br> Linux – alle Linux agenten  <br> AzureStorage – Azure diagnostische gegevens |
| TimeGenerated       | Datum en tijd waarop die de gebeurtenis is gemaakt in Windows. |
| Gebruikersnaam            | De gebruikersnaam van het account dat de gebeurtenis is vastgelegd. |



## <a name="log-searches-with-windows-events"></a>Zoekopdrachten in het logboek met Windows-gebeurtenissen

De volgende tabel bevat verschillende voorbeelden van log zoekopdrachten die Windows gebeurtenis records ophalen.

| Query | Beschrijving |
|:--|:--|
| Typ = gebeurtenis | Alle gebeurtenissen van Windows. |
| Typ = gebeurtenis EventLevelName = fout | Alle gebeurtenissen van Windows met ernst van fout. |
| Typ = gebeurtenis & #124; Eenheid count() door de bron | Tellen van Windows-gebeurtenissen op bron. |
| Typ = gebeurtenis EventLevelName = fout & #124; Eenheid count() door de bron | Telling van Windows foutgebeurtenissen op bron. |

## <a name="next-steps"></a>Volgende stappen

- Log Analytics te verzamelen van andere [gegevensbronnen](log-analytics-data-sources.md) voor analyse configureren.
- Meer informatie over [log zoekopdrachten](log-analytics-log-searches.md) om de gegevens die worden verzameld uit gegevensbronnen en oplossingen te analyseren.  
- [Aangepaste velden](log-analytics-custom-fields.md) gebruiken om te parseren van de gebeurtenisrecords in afzonderlijke velden.
- Configureer de [verzameling prestatiemeteritems](log-analytics-data-sources-performance-counters.md) van uw Windows-agenten.