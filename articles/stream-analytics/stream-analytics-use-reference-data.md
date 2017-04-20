<properties
    pageTitle="Tabellen met gegevens en opzoeken gebruiken in Stream Analytics | Microsoft Azure"
    description="Verwijzingsgegevens gebruiken in een query Stream Analytics"
    keywords="opzoektabel, referentiegegevens"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="using-reference-data-or-lookup-tables-in-a-stream-analytics-input-stream"></a>Verwijzing gegevens of de opzoektabel-tabellen gebruiken in een Stream Analytics invoer stream

Referentiegegevens (ook wel bekend als een opzoektabel) is een eindige gegevensverzameling die is statische of vertragen wijzigt in aard, gebruikt wilt opzoeken of correlatie met de stroom van uw gegevens. Gebruik van referentiegegevens in uw Azure Stream Analytics-taak die u in het algemeen gebruikt een [Verwijzing gegevens deelnemen](https://msdn.microsoft.com/library/azure/dn949258.aspx) in uw Query. Stream Analytics Azure-blobopslag als de opslaglaag voor referentiegegevens gebruikt en met Azure gegevens Factory verwijzing gegevens kunnen worden getransformeerd en/of gekopieerd naar Azure-blobopslag, voor gebruik als verwijzingsgegevens uit [een willekeurig aantal cloud gebaseerd en on-premises gegevens winkels](../data-factory/data-factory-data-movement-activities.md). Referentiegegevens is als een reeks BLOB's (gedefinieerd in de configuratie van de invoer) in oplopende volgorde van de datum/tijd opgegeven in de naam van de blob gebaseerd. Dit ondersteunt **alleen** toevoegen aan het einde van de reeks met behulp van een datum/tijd **groter is** dan het opgegeven door de laatste label in de reeks.

## <a name="configuring-reference-data"></a>Referentiegegevens configureren

Als u wilt uw referentiegegevens configureren, moet u eerst een invoer die van het type **Referentiegegevens**maken. De onderstaande tabel wordt uitgelegd elke eigenschap die u nodig hebt bij het maken van de verwijzing gegevensinvoer met een beschrijving opgeven:

<table>
<tbody>
<tr>
<td>Naam van eigenschap</td>
<td>Beschrijving</td>
</tr>
<tr>
<td>Invoer Alias</td>
<td>Een beschrijvende naam die wordt gebruikt in de query taak om te verwijzen naar deze invoer.</td>
</tr>
<tr>
<td>Opslag-Account</td>
<td>De naam van de opslag-account waarin uw blob-bestanden opgeslagen worden. Als hetzelfde als de taak van de Stream Analytics-abonnement is, kunt u deze selecteren in de vervolgkeuzelijst omlaag.</td>
</tr>
<tr>
<td>Accountsleutel opslag</td>
<td>De geheime sleutel die is gekoppeld aan het account opslag. Hiermee wordt automatisch ingevuld als de opslagruimte-account hetzelfde als de taak van de Stream Analytics-abonnement is.</td>
</tr>
<tr>
<td>Opslag Container</td>
<td>Containers bieden een logische groepering voor BLOB's die zijn opgeslagen in de service Microsoft Azure Blob. Wanneer u een blob naar de Blob-service uploaden, moet u een container voor die blob.</td>
</tr>
<tr>
<td>Pad patroon</td>
<td>Het bestandspad gebruikt om te zoeken van uw BLOB's binnen de opgegeven container. U kunt kiezen in het pad, kunt u een of meer exemplaren van de volgende 2 variabelen opgeven:<BR>{date}, {tijd}<BR>Voorbeeld 1: products/{date}/{time}/product-list.csv<BR>Voorbeeld 2: products/{date}/product-list.csv
</tr>
<tr>
<td>Datumnotatie [optionele]</td>
<td>Als u {date} binnen het pad patroon dat u hebt opgegeven gebruikt hebt, kunt u de datumnotatie waarin uw bestanden worden opgeslagen in de vervolgkeuzelijst van de ondersteunde indelingen selecteren. Voorbeeld: Jjjj/MM-DD</td>
</tr>
<tr>
<td>Tijdnotatie [optionele]</td>
<td>Als u {tijd} binnen het pad patroon dat u hebt opgegeven gebruikt hebt, kunt u de tijdnotatie waarin uw bestanden worden opgeslagen in de vervolgkeuzelijst van de ondersteunde indelingen selecteren. Voorbeeld: [HH</td>
</tr>
<tr>
<td>Gebeurtenis serialisatie-indeling</td>
<td>Om ervoor te zorgen dat uw query's werkt zoals verwacht, Stream Analytics moet weten welke serialiseringsindeling u gebruikt voor binnenkomende gegevensstromen. Voor referentiegegevens zijn de ondersteunde indelingen CSV- en JSON.</td>
</tr>
<tr>
<td>Codering</td>
<td>UTF-8 is de enige ondersteunde codering indeling op dit moment</td>
</tr>
</tbody>
</table>

## <a name="generating-reference-data-on-a-schedule"></a>Referentiegegevens volgens een schema genereren

Als uw verwijzingsgegevens een langzaam veranderende gegevensverzameling is, klikt u vervolgens de ondersteuning voor het vernieuwen van gegevens is ingeschakeld door op te geven van een patroon pad in de invoer configuratie met de {datum} verwijzing en vervanging tokens {afstemmen}. Stream Analytics wordt verdergaan met de bijgewerkte verwijzing gegevensdefinities op basis van dit pad patroon. Bijvoorbeeld een patroon van `sample/{date}/{time}/products.csv` met een datumnotatie **"jjjj-MM-DD"** tijd en een indeling van **"Uu: mm"** Hiermee geeft u voor Stream Analytics volgt te werk om de bijgewerkte blob `sample/2015-04-16/17:30/products.csv` bij 5:30 PM op April 16de 2015 UTC-tijdzone.

> [AZURE.NOTE] Momenteel zoekt Stream Analytics taken u het vernieuwen blob alleen wanneer de systeemtijd naar de tijd in de naam van de blob codering verplaatst. Bijvoorbeeld de taak wordt gezocht naar `sample/2015-04-16/17:30/products.csv` zo snel mogelijk, maar geen eerder dan 5:30 PM op April 16de 2015 UTC tijdzone in. Deze worden *nooit* uiterlijk voor een bestand met een gecodeerd tijd eerder dan de laatste tabel die is gevonden.
> 
> Bijvoorbeeld Nadat de taak vindt u de blob `sample/2015-04-16/17:30/products.csv` deze bestanden met een gecodeerd datum eerder dan 5:30 PM April 16de 2015 negeert dus als u een late binnengekomen `sample/2015-04-16/17:25/products.csv` blob wordt gemaakt in dezelfde container de taak wordt niet gebruiken.
> 
> Op dezelfde manier als `sample/2015-04-16/17:30/products.csv` is alleen bij 10:03 PM April 16de 2015 geproduceerd maar geen blob met een eerdere datum in de container aanwezig is, wordt de taak wordt dit bestand vanaf 10:03 PM April 16de 2015 gebruiken en gebruik van de vorige referentiegegevens tot typt u vervolgens.
> 
> Een uitzondering hierop is wanneer de taak opnieuw verwerken gegevens terug in tijd moet of wanneer de taak eerste is gestart. De taak wordt gezocht naar de meest recente blob geproduceerd voordat u de taak begintijd begin tijd die is opgegeven. Dit gebeurt om ervoor te zorgen dat er een **niet-lege** verwijzing gegevensverzameling is wanneer de taak wordt gestart. Als een kan niet worden gevonden, de taak weer om de volgende diagnostische: `Initializing input without a valid reference data blob for UTC time <start time>`.


[Azure gegevens Factory](https://azure.microsoft.com/documentation/services/data-factory/) kan worden gebruikt de taak voor het maken van de bijgewerkte BLOB's Stream Analytics vereist om het bijwerken van de verwijzing gegevensdefinities van goedkeuringen. Gegevens Factory is een cloudgebaseerde integratie gegevensservice die orchestrates en automatiseert de verplaatsing en transformatie van gegevens. Gegevens Factory kunt u [verbinding maakt met een groot aantal cloud gebaseerd en on-premises implementatie-gegevensopslag](../data-factory/data-factory-data-movement-activities.md) en zwevend gegevens eenvoudig regelmatig die u opgeeft. Voor meer informatie en stapsgewijze instructies over het instellen van een pijplijn Data Factory om verwijzingsgegevens te genereren voor Stream Analytics die worden vernieuwd op een vooraf gedefinieerde planning, raadpleegt u deze [GitHub voorbeeld](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

## <a name="tips-on-refreshing-your-reference-data"></a>Tips over het vernieuwen van uw verwijzingsgegevens ##

1. Verwijzing gegevens BLOB's overschrijven veroorzaakt niet Stream Analytics opnieuw laden de blob en in sommige gevallen kan het gebeuren dat de taak mislukt. De aanbevolen wijze referentiegegevens wijzigen is een nieuwe blob met dezelfde container toevoegen en pad patroon in de invoer van de taak gedefinieerd en gebruikt u een datum/tijd **groter is** dan het opgegeven door de laatste label in de reeks.
2.  Referentiegegevens BLOB's worden niet geordend door van de blob "Laatst gewijzigd" tijd, maar alleen door de tijd en datum die is opgegeven in de label een naam geven met de {datum} en {afstemmen} vervangen.
3.  Klik op een paar gelegenheden die een taak u terug in tijd gaat moet, moeten dus verwijzing gegevens BLOB's niet worden gewijzigd of verwijderd.

## <a name="get-help"></a>U kunt hulp krijgen
Voor hulp, probeert u onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
U hebt nu enkele Stream Analytics, een beheerde service voor streaming analytics op gegevens uit de Internet van zaken aan bod. Meer informatie over deze service, raadpleegt u:

- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
