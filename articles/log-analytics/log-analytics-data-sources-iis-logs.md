<properties
   pageTitle="IIS-logboeken in Log Analytics | Microsoft Azure"
   description="Internet Information Services (IIS) slaat gebruikers actief in de logboekbestanden die kunnen worden verzameld door Log Analytics.  In dit artikel wordt beschreven hoe verzameling IIS-logboeken en details van de records die ze in de bibliotheek OMS maken configureren."
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

# <a name="iis-logs-in-log-analytics"></a>IIS-logboeken in Log Analytics
Internet Information Services (IIS) slaat gebruikers actief in de logboekbestanden die kunnen worden verzameld door Log Analytics.  

![IIS-logboeken](media/log-analytics-data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Logboeken IIS configureren
Log Analytics Hiermee vermeldingen verzameld uit de logboekbestanden die door IIS, zodat u [IIS voor logboekregistratie configureren moet](https://technet.microsoft.com/library/hh831775.aspx).

Log Analytics IIS-logboekbestanden die zijn opgeslagen in de W3C-indeling ondersteunt alleen en biedt geen ondersteuning voor aangepaste velden of IIS geavanceerde logboekregistratie.  
Log Analytics verzamelt geen logboeken in systeemeigen NCSA of IIS-indeling.

IIS-logboeken in Log Analytics in het [menu van de gegevens in Log Analytics-instellingen](log-analytics-data-sources.md#configuring-data-sources)configureren.  Er is geen configuratie vereist andere dan **verzamelen W3C-indeling IIS-logboekbestanden**te selecteren.

Het is raadzaam wanneer u IIS log siteverzameling inschakelt, u de instelling IIS log rollover op elke server configureren moet.


## <a name="data-collection"></a>Gegevens verzamelen

Log Analytics verzamelt IIS-logboekvermeldingen van elke verbonden bron ongeveer elke 15 minuten.  De positie records de agent in elke gebeurtenislogboek die worden verzameld uit.  Als de agent offline gaat, klikt u vervolgens verzamelt Log Analytics gebeurtenissen van waar deze laatste gebleven was, zelfs als de gebeurtenissen die zijn gemaakt terwijl de-agent offline is.


## <a name="iis-log-record-properties"></a>IIS log recordeigenschappen

IIS-logboekrecords een soort **W3CIISLog** en hebt de eigenschappen in de volgende tabel:

| Eigenschap | Beschrijving |
|:--|:--|
| Computer | Naam van de computer waarop de gebeurtenis is verzameld uit. |
| cIP | IP-adres van de client. |
| csMethod | Methode van de aanvraag zoals GET of POST. |
| csReferer | De site die de gebruiker een koppeling uit naar de huidige site gevolgd. |
| csUserAgent | Het type van de browser van de client. |
| csUserName | De naam van de geverifieerde gebruiker die de server gebruikt. Anonieme gebruikers worden aangeduid met een afbreekstreepje. |
| csUriStem | Doel van de aanvraag zoals een webpagina. |
| csUriQuery | Een query uitvoert, indien van toepassing, dat de client heeft geprobeerd om uit te voeren. |
| ManagementGroupName | De naam van de management group voor Operations Manager agenten.  Voor andere gemachtigden, is dit de AOI -\<werkruimte-ID\> |
| RemoteIPCountry | Land van het IP-adres van de client. |
| RemoteIPLatitude | Breedtegraad van het IP-adres van client. |
| RemoteIPLongitude | De lengte van het IP-adres van client. |
| scStatus | HTTP-statuscode. |
| scSubStatus | Onderliggende fout statuscode. |
| scWin32Status | Windows-statuscode. |
| sIP | IP-adres van de webserver. |
| SourceSystem  | OpsMgr |
| sPort | Poort op de server de client die is gekoppeld aan. |
| sSiteName | De naam van de IIS-site. |
| TimeGenerated | Datum en tijd die het fragment is geregistreerd. |
| TimeTaken | Periode verwerking van de aanvraag (in milliseconden). |

## <a name="log-searches-with-iis-logs"></a>Zoekopdrachten in het logboek met IIS-logboeken

De volgende tabel bevat verschillende voorbeelden van log-query's die IIS-logboekrecords ophalen.

| Query | Beschrijving |
|:--|:--|
| Typ = IISLog | Alle IIS-logboekrecords. |
| Typ = IISLog EventLevelName = fout | Alle gebeurtenissen van Windows met ernst van fout. |
| Typ = W3CIISLog & #124; Count() door cIP meten | Telling van IIS-logboekvermeldingen door IP-adres van client. |
| Typ = W3CIISLog csHost = "www.contoso.com" & #124; Count() door csUriStem meten | Tellen van IIS logboekvermeldingen door de URL voor de host www.contoso.com. |
| Typ = W3CIISLog & #124; Eenheid Sum(csBytes) Computer & #124; bovenste 500000| Totaal aantal bytes dat is ontvangen door elke IIS-computer. |

## <a name="next-steps"></a>Volgende stappen

- Log Analytics te verzamelen van andere [gegevensbronnen](log-analytics-data-sources.md) voor analyse configureren.
- Meer informatie over [log zoekopdrachten](log-analytics-log-searches.md) om de gegevens die worden verzameld uit gegevensbronnen en oplossingen te analyseren.
- Waarschuwingen in Log Analytics te informeren u belangrijke voorwaarden die zijn gevonden in IIS-logboeken configureren.
