<properties
    pageTitle="Streamen Analytics-uitvoer: opties voor de opslag, analyse | Microsoft Azure"
    description="Meer informatie over opties voor uitvoer van Stream Analytics gegevens inclusief Power BI voor analyseresultaten doelgroepen."
    keywords="gegevenstransformatie, analyseresultaten, opslagopties van gegevens"
    services="stream-analytics,documentdb,sql-database,event-hubs,service-bus,storage"
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

# <a name="stream-analytics-outputs-options-for-storage-analysis"></a>Streamen Analytics-uitvoer: opties voor de opslag, analyse

Tijdens het ontwerpen van een taak Stream analyses, kunt u hoe de resulterende gegevens wordt verbruikt. Hoe wordt u de resultaten van de Stream Analytics-taak bekijken en waar wilt u deze opslaan?

Om te kunnen inschakelen tal van toepassing patronen, heeft Azure Stream Analytics verschillende opties voor het opslaan van uitvoer en analyseresultaten weergeven. Hiermee kunt u heel gemakkelijk taak uitvoer weergeven en krijgt u flexibiliteit bij het verbruik en opslag van de taak-uitvoer voor gegevensopslag en voor andere doeleinden. Uitvoer geconfigureerd in de taak moet bestaan voordat de taak is gestart en gebeurtenissen die doorloopt. Bijvoorbeeld als u blobopslag als een uitvoer gebruikt, maakt de taak niet een opslag-account automatisch. Deze moet worden gemaakt door de gebruiker om de taak ASA is gestart.

## <a name="azure-data-lake-store"></a>Azure Lake gegevensopslag

Stream Analytics ondersteunt [Azure Lake gegevensopslag](https://azure.microsoft.com/services/data-lake-store/). Deze opslag kunt u gegevens van elke grootte, type en opname snelheid van operationele en experimentele analysegegevens op te slaan. Op dit moment kan wordt maken en de configuratie van Lake gegevensopslag uitvoer alleen ondersteund in de klassieke Azure-Portal. Stream Analytics moet bovendien toegang heeft tot de Lake gegevensopslag. Meer informatie over autorisatie en hoe u zich aanmeldt voor het voorbeeld van gegevens Lake Store (indien nodig) worden besproken in de [gegevens Lake uitvoer artikel](stream-analytics-data-lake-output.md).

### <a name="authorize-an-azure-data-lake-store"></a>Autoriseer een Lake van Azure gegevensopslag

Wanneer Lake gegevensopslag als een uitvoer in de beheerportal van Azure is geselecteerd, wordt u gevraagd een verbinding met een bestaande Lake gegevensopslag toe te staan.  

![Autoriseer Lake gegevensopslag](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

Vul vervolgens de eigenschappen voor de uitvoer Lake gegevensopslag zoals hieronder wordt weergegeven:

![Autoriseer Lake gegevensopslag](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

De onderstaande tabel bevat de eigenschapnamen en de bijbehorende beschrijvingen die u nodig hebt voor het maken van een uitvoer Lake gegevensopslag.

<table>
<tbody>
<tr>
<td><B>NAAM VAN EIGENSCHAP</B></td>
<td><B>BESCHRIJVING</B></td>
</tr>
<tr>
<td>Uitvoeralias</td>
<td>Dit is een beschrijvende naam in query's gebruikt om de queryuitvoer naar deze gegevens Lake Store.</td>
</tr>
<tr>
<td>Accountnaam</td>
<td>De naam van het Lake gegevensopslag account waar u uw uitvoer verzendt. U krijgt een vervolgkeuzelijst Lake gegevensopslag accounts waaraan de gebruiker is aangemeld bij de portal toegang tot heeft.</td>
</tr>
<tr>
<td>Pad voorvoegsel patroon [<I>optionele</I>]</td>
<td>Het bestandspad gebruikt om te schrijven van uw bestanden in de opgegeven gegevens Lake Store-Account. <BR>{date}, {tijd}<BR>Voorbeeld 1: Map1/logboeken / {date} / {afstemmen}<BR>Voorbeeld 2: Map1/logboeken / {date}</td>
</tr>
<tr>
<td>Datumnotatie [<I>optionele</I>]</td>
<td>Als het token datum wordt gebruikt in het voorvoegsel pad, kunt u de datumnotatie waarin uw bestanden worden opgeslagen. Voorbeeld: Jjjj/MM-DD</td>
</tr>
<tr>
<td>Tijdnotatie [<I>optionele</I>]</td>
<td>Als de tijd token wordt gebruikt in het voorvoegsel pad, geeft u de tijdnotatie waarin uw bestanden worden opgeslagen. De enige ondersteunde waarde is momenteel [HH.</td>
</tr>
<tr>
<td>Gebeurtenis serialisatie-indeling</td>
<td>Serialiseringsindeling voor uitvoergegevens. JSON, CSV- en Avro worden ondersteund.</td>
</tr>
<tr>
<td>Codering</td>
<td>Als CSV- of JSON opmaken, moet een codering worden opgegeven. De enige ondersteunde codering indeling is UTF-8 op dit moment.</td>
</tr>
<tr>
<td>Een scheidingsteken</td>
<td>Alleen van toepassing op CSV-serialisatie. Stream Analytics ondersteunt een aantal veelvoorkomende scheidingstekens voor serialiseren CSV-gegevens. Ondersteunde waarden zijn komma, puntkomma, spatie, tab en verticale balk.</td>
</tr>
<tr>
<td>Indeling</td>
<td>Alleen van toepassing op JSON serialisatie. Gescheiden geeft aan dat de uitvoer doordat elk JSON-object gescheiden door een nieuwe regel worden opgemaakt. Matrix geeft aan dat de uitvoer worden opgemaakt als een matrix van JSON objecten.</td>
</tr>
</tbody>
</table>

### <a name="renew-data-lake-store-authorization"></a>Autorisatie voor gegevensopslag Lake verlengen

Moet u uw account voor gegevensopslag Lake opnieuw worden geverifieerd als het wachtwoord heeft gewijzigd sinds de taak is gemaakt of laatst geverifieerd.

![Autoriseer Lake gegevensopslag](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  


## <a name="sql-database"></a>SQL-Database

[Azure SQL-Database](https://azure.microsoft.com/services/sql-database/) kunnen worden gebruikt als een uitvoer voor gegevens die in aard relationele of voor toepassingen die afhankelijk zijn van inhoud die wordt gehost in een relationele database. Stream Analytics taken schrijft aan een bestaande tabel in een Azure SQL-Database.  Houd er rekening mee dat het tabelschema moet precies overeenkomen met de velden en hun bestandstypen uitvoer van uw taak. Een [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) kan ook worden opgegeven als een uitvoer via de SQL-Database uitvoeroptie ook (dit is een voorbeeldfunctie). De onderstaande tabel bevat de eigenschapnamen en de bijbehorende beschrijvingen voor het maken van de uitvoer van een SQL-Database.

| Naam van eigenschap | Beschrijving |
|---------------|-------------|
| Uitvoeralias | Dit is een beschrijvende naam in query's gebruikt om de queryuitvoer met deze database. |
| Database | De naam van de database waarin u uw uitvoer verstuurt |
| De naam van server | De naam van de SQL-Database-server |
| Gebruikersnaam | De gebruikersnaam die toegang heeft tot de database schrijven |
| Wachtwoord | Het wachtwoord voor het verbinding maken met de database |
| Tabel | De naam van de tabel waar de uitvoer worden geschreven. De naam van de tabel is hoofdlettergevoelig en het schema van deze tabel exact op het aantal velden en hun typen wordt gegenereerd door de uitvoer van uw project moet overeenkomen. |

> [AZURE.NOTE] De aanbod Azure SQL-Database wordt momenteel ondersteund voor de uitvoer van een taak in Stream Analytics. Een Azure virtuele machines met SQL Server met een database die gekoppeld zijn, wordt niet ondersteund. Dit kan worden gewijzigd in toekomstige versies.

## <a name="blob-storage"></a>-Blobopslag

Blobopslag biedt een efficiënt en scalable oplossing voor het opslaan van grote hoeveelheden ongestructureerde gegevens in de cloud.  Zie de documentatie bij [het gebruik van BLOB's](../storage/storage-dotnet-how-to-use-blobs.md)voor een inleiding op Azure-blobopslag en het gebruik ervan.

De onderstaande tabel bevat de eigenschapnamen en de bijbehorende beschrijvingen voor het maken van een blob-uitvoer.

<table>
<tbody>
<tr>
<td>NAAM VAN EIGENSCHAP</td>
<td>BESCHRIJVING</td>
</tr>
<tr>
<td>Uitvoeralias</td>
<td>Dit is een beschrijvende naam in query's gebruikt om de queryuitvoer met deze-blobopslag.</td>
</tr>
<tr>
<td>Opslag-Account</td>
<td>De naam van de opslag-account waar u uw uitvoer verzendt.</td>
</tr>
<tr>
<td>Accountsleutel opslag</td>
<td>De geheime sleutel die is gekoppeld aan het account opslag.</td>
</tr>
<tr>
<td>Opslag Container</td>
<td>Containers bieden een logische groepering voor BLOB's die zijn opgeslagen in de service Microsoft Azure Blob. Wanneer u een blob naar de Blob-service uploaden, moet u een container voor die blob.</td>
</tr>
<tr>
<td>Pad voorvoegsel patroon [optionele]</td>
<td>Het bestandspad gebruiken om uw BLOB's binnen de opgegeven container schrijven.<BR>Binnen het pad, kunt u kiezen of u wilt een of meer exemplaren van de volgende 2 variabelen gebruiken voor de frequentie die BLOB's zijn geschreven:<BR>{date}, {tijd}<BR>Voorbeeld 1: cluster1/logboeken / {date} / {afstemmen}<BR>Voorbeeld 2: cluster1/logboeken / {date}</td>
</tr>
<tr>
<td>Datumnotatie [optionele]</td>
<td>Als het token datum wordt gebruikt in het voorvoegsel pad, kunt u de datumnotatie waarin uw bestanden worden opgeslagen. Voorbeeld: Jjjj/MM-DD</td>
</tr>
<tr>
<td>Tijdnotatie [optionele]</td>
<td>Als de tijd token wordt gebruikt in het voorvoegsel pad, geeft u de tijdnotatie waarin uw bestanden worden opgeslagen. De enige ondersteunde waarde is momenteel [HH.</td>
</tr>
<tr>
<td>Gebeurtenis serialisatie-indeling</td>
<td>Serialiseringsindeling voor uitvoergegevens.  JSON, CSV- en Avro worden ondersteund.</td>
</tr>
<tr>
<td>Codering</td>
<td>Als CSV- of JSON opmaken, moet een codering worden opgegeven. De enige ondersteunde codering indeling is UTF-8 op dit moment.</td>
</tr>
<tr>
<td>Een scheidingsteken</td>
<td>Alleen van toepassing op CSV-serialisatie. Stream Analytics ondersteunt een aantal veelvoorkomende scheidingstekens voor serialiseren CSV-gegevens. Ondersteunde waarden zijn komma, puntkomma, spatie, tab en verticale balk.</td>
</tr>
<tr>
<td>Indeling</td>
<td>Alleen van toepassing op JSON serialisatie. Gescheiden geeft aan dat de uitvoer doordat elk JSON-object gescheiden door een nieuwe regel worden opgemaakt. Matrix geeft aan dat de uitvoer worden opgemaakt als een matrix van JSON objecten.</td>
</tr>
</tbody>
</table>

## <a name="event-hub"></a>Gebeurtenis-Hub

[Gebeurtenis Hubs](https://azure.microsoft.com/services/event-hubs/) is een zeer scalable publiceren abonnementen gebeurtenis ingestor. Dit kan miljoenen gebeurtenissen per seconde verzamelen.  Eén gebruik van de Hub van een gebeurtenis als uitvoer is wanneer de uitvoer van een taak Stream Analytics de invoer van een andere streaming taak worden.

Er zijn een paar parameters die nodig zijn voor het configureren van gebeurtenis Hub gegevensstromen als een uitvoer.

| Naam van eigenschap | Beschrijving |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Uitvoeralias | Dit is een beschrijvende naam in query's gebruikt om de queryuitvoer op deze gebeurtenis-Hub. |
| Service Bus Namespace | Een Service Bus naamruimte is een container voor een reeks entiteiten messaging. Wanneer u een nieuwe gebeurtenis Hub hebt gemaakt, u ook een naamruimte Service Bus gemaakt |
| Gebeurtenis-Hub | De naam van de gebeurtenis Hub-uitvoer |
| De naam van een gebeurtenis Hub-beleid | Het gedeelde-beleid, die kan worden gemaakt op het tabblad gebeurtenis Hub configureren. Elk gedeelde-beleid wordt een naam hebben, machtigingen die u hebt ingesteld, en de toegangstoetsen |
| Gebeurtenis-Hub beleid-toets | De toegang tot gedeelde sleutel gebruikt om te verifiëren toegang tot de Service Bus naamruimte |
| Partition sleutelkolom [optionele] | Deze kolom bevat de partitiesleutel voor gebeurtenis Hub uitvoer. |
| Gebeurtenis serialisatie-indeling | Serialiseringsindeling voor uitvoergegevens.  JSON, CSV- en Avro worden ondersteund. |
| Codering | Voor de CSV- en JSON is UTF-8 de enige ondersteunde codering indeling op dit moment |
| Een scheidingsteken | Alleen van toepassing op CSV-serialisatie. Stream Analytics ondersteunt een aantal veelvoorkomende scheidingstekens voor serialiseren gegevens in een CSV-indeling. Ondersteunde waarden zijn komma, puntkomma, spatie, tab en verticale balk. |
| Indeling | Alleen van toepassing op type JSON. Gescheiden geeft aan dat de uitvoer doordat elk JSON-object gescheiden door een nieuwe regel worden opgemaakt. Matrix geeft aan dat de uitvoer worden opgemaakt als een matrix van JSON objecten. |

## <a name="power-bi"></a>Power BI

[Power BI](https://powerbi.microsoft.com/) kunnen worden gebruikt als een uitvoer voor een taak Stream Analytics verstrekken voor een uitgebreide visualisatie-ervaring van analyseresultaten. Deze functie kan worden gebruikt voor operationele dashboards, rapporten en metrisch rapporteren basis.

### <a name="authorize-a-power-bi-account"></a>Autoriseer een Power BI-account

1.  Wanneer u Power BI als een uitvoer in de beheerportal van Azure selecteert, wordt u gevraagd naar Autoriseer van een bestaande Power BI-gebruiker of naar een nieuwe Power BI-account maken.  

    ![Power BI-gebruiker toestaan](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  

2.  Een nieuw account maken als u niet nog een hebt en klik vervolgens op nu Autoriseer.  Een scherm zoals de volgende wordt gepresenteerd.  

    ![Azure-Account Power BI](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  

3.  Geef het account voor werk- of schoolaccount om de Power BI-uitvoer te machtigen in deze stap. Als u worden niet al hebben geregistreerd voor Power BI, kiest u nu teken omhoog. Het werk of school account dat u voor Power BI gebruikt kan afwijken van de Azure abonnementaccount waarmee u zich hebt aangemeld met.

### <a name="configure-the-power-bi-output-properties"></a>De eigenschappen van het Power BI-uitvoer configureren

Nadat u het Power BI-account dat is geverifieerd hebt, kunt u de eigenschappen voor uw Power BI-uitvoer configureren. De onderstaande tabel is de lijst met eigenschapnamen en de bijbehorende beschrijvingen voor het configureren van uw Power BI-uitvoer.

| Naam van eigenschap | Beschrijving |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Uitvoeralias | Dit is een beschrijvende naam in query's gebruikt om de queryuitvoer naar deze PowerBI-uitvoer. |
| Groep-werkruimte | Als u van delen van gegevens met andere gebruikers Power BI kunt u groepen selecteren in uw Power BI-account of kies 'Mijn werkruimte' als u niet wilt schrijven naar een groep.  Bijwerken van een bestaande groep is vereist voor het vernieuwen van de Power BI-verificatie. | 
| De naam van de gegevensset | Geef de gegevenssetnaam van een dat dit nodig is voor de Power BI-uitvoer gebruiken |
| Tabelnaam | Geef de naam van een tabel onder de gegevensset van de Power BI-uitvoer. Op dit moment kan Power BI-uitvoer van Stream Analytics taken alleen hebben één tabel in een gegevensset |

Voor een overzicht van het configureren van een Power BI-uitvoer en dashboard, raadpleegt u het artikel [Azure Stream Analytics en Power BI](stream-analytics-power-bi-dashboard.md) .

> [AZURE.NOTE] Niet expliciet Maak de gegevensset en de tabel in het Power BI-dashboard. De gegevensset en de tabel wordt automatisch ingevuld wanneer de taak is gestart en het project reageert uitvoer in Power BI begint. Houd er rekening mee dat als de query taak geen resultaten worden genereren, de gegevensset en tabel niet worden gemaakt. Bedenk ook dat als Power BI al een gegevensset en een tabel met dezelfde naam als het een die beschikbaar zijn in deze taak Stream analyses, de bestaande gegevens overschreven.

### <a name="renew-power-bi-authorization"></a>Power BI autorisatie verlengen

U moet uw Power BI-account opnieuw te verifiëren als het wachtwoord heeft gewijzigd sinds de taak is gemaakt of laatst geverifieerd. U moet dus ook Power BI autorisatie voor elke 2 weken verlengen als meervoudige verificatie MFA () is geconfigureerd op uw tenant Azure Active Directory (AAD). Een symptoom van dit probleem is geen uitvoer taak en een 'verifiëren gebruikersfout' in de logboeken aan de bewerking:

  ![Power BI vernieuwen token fout](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

U lost dit probleem op door uw actieve taak stoppen en Ga naar uw Power BI-uitvoer.  Klik op de koppeling "Verlengen autorisatie" en uw taak van de laatste keer dat kan worden gestopt om te voorkomen van gegevensverlies opnieuw starten.

  ![Power BI autorisatie verlengen](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Table Storage

[Azure-tabelopslag](../storage/storage-introduction.md) biedt ten zeerste beschikbaar, sterk schaalbare opslag, zodat de schaal van een toepassing automatisch aanpassen kunt om te voldoen aan de gebruikersvraag. Tabelopslag is Microsofts NoSQL sleutel/kenmerk store welke monitor van gestructureerde gegevens met minder beperkingen bij het schema gebruikmaken kunt. Azure-tabelopslag kan worden gebruikt voor de opslag van gegevens voor permanente en efficiënt ophalen.

De onderstaande tabel bevat de eigenschapnamen en de bijbehorende beschrijvingen voor het maken van de uitvoer van een tabel.

| Naam van eigenschap | Beschrijving |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Uitvoeralias | Dit is een beschrijvende naam in query's gebruikt om de queryuitvoer met deze tabelopslag. |
| Opslag-Account | De naam van de opslag-account waar u uw uitvoer verzendt. |
| Accountsleutel opslag | De access-sleutel die is gekoppeld aan het account opslag. |
| Tabelnaam | De naam van de tabel. De tabel gemaakt als deze nog niet bestaat. |
| Partition-toets | De naam van de uitvoerkolom die de partitiesleutel bevat. De partitiesleutel is een unieke id voor de partition binnen een bepaalde tabel die het eerste deel van de primaire sleutel van een eenheid vormt. Dit is een tekenreekswaarde die deel kan maximaal 1 KB grootte. |
| Rij-toets | De naam van de uitvoerkolom met de rijsleutel. De rijsleutel is een unieke id voor een entiteit binnen een bepaald partition. Het tweede deel van de primaire sleutel van een eenheid vormt. De rijsleutel is een tekenreekswaarde die deel kan maximaal 1 KB grootte. |
| Batchgrootte | Het aantal records voor een batchbewerking. Meestal de standaardinstelling is voldoende voor de meeste taken, verwijst naar de [tabel batchbewerking specificaties](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) voor meer informatie over het wijzigen van deze instelling. |

## <a name="service-bus-queues"></a>Service Bus wachtrijen

[Service Bus wachtrijen](https://msdn.microsoft.com/library/azure/hh367516.aspx) bieden een eerste keer hebt aangemeld, berichtbezorging eerste Out (FIFO) naar een of meer concurrerende consumenten. Meestal berichten verwachting worden ontvangen en verwerkt door de ontvangers in de tijdelijke volgorde waarin ze zijn toegevoegd aan de wachtrij en elk bericht is ontvangen en verwerkt door consumenten slechts één bericht.

De onderstaande tabel bevat de eigenschapnamen en de bijbehorende beschrijvingen voor het maken van een wachtrij-uitvoer.

| Naam van eigenschap | Beschrijving |
|----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Uitvoeralias | Dit is een beschrijvende naam in query's gebruikt om de queryuitvoer naar deze Service Bus wachtrij. |
| Service Bus Namespace | Een Service Bus naamruimte is een container voor een reeks entiteiten messaging. |
| De wachtrijnaam van de | De naam van de Service Bus wachtrij. |
| De naam van de wachtrij-beleid | Wanneer u een wachtrij maakt, kunt u ook gedeelde-beleid maken op het tabblad wachtrij configureren. Elk gedeelde-beleid wordt een naam hebben, machtigingen die u hebt ingesteld, en de toegangstoetsen. |
| Wachtrij beleid-toets | De toegang tot gedeelde sleutel gebruikt om te verifiëren toegang tot de Service Bus naamruimte |
| Gebeurtenis serialisatie-indeling | Serialiseringsindeling voor uitvoergegevens.  JSON, CSV- en Avro worden ondersteund. |
| Codering | Voor de CSV- en JSON is UTF-8 de enige ondersteunde codering indeling op dit moment |
| Een scheidingsteken | Alleen van toepassing op CSV-serialisatie. Stream Analytics ondersteunt een aantal veelvoorkomende scheidingstekens voor serialiseren gegevens in een CSV-indeling. Ondersteunde waarden zijn komma, puntkomma, spatie, tab en verticale balk. |
| Indeling | Alleen van toepassing op type JSON. Gescheiden geeft aan dat de uitvoer doordat elk JSON-object gescheiden door een nieuwe regel worden opgemaakt. Matrix geeft aan dat de uitvoer worden opgemaakt als een matrix van JSON objecten. |

## <a name="service-bus-topics"></a>Service Bus onderwerpen

Service Bus wachtrijen zorgen voor een één communicatiemethode van afzender aan de ontvanger, vindt [Service Bus onderwerpen](https://msdn.microsoft.com/library/azure/hh367516.aspx) u een een-op-veel-formulier van communicatie.

De onderstaande tabel bevat de eigenschapnamen en de bijbehorende beschrijvingen voor het maken van de uitvoer van een tabel.

| Naam van eigenschap | Beschrijving |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Uitvoeralias | Dit is een beschrijvende naam in query's gebruikt om de queryuitvoer naar deze Service Bus onderwerp. |
| Service Bus Namespace | Een Service Bus naamruimte is een container voor een reeks entiteiten messaging. Wanneer u een nieuwe gebeurtenis Hub hebt gemaakt, u ook een naamruimte Service Bus gemaakt |
| De onderwerpnaam van het | Onderwerpen zijn entiteiten, vergelijkbaar met gebeurtenis hubs en wachtrijen messaging. Ze bent ontworpen voor het verzamelen van gebeurtenis streams uit een aantal verschillende apparaten en -services. Wanneer een onderwerp is gemaakt, krijgt deze ook een specifieke naam. De berichten voor een onderwerp is alleen beschikbaar als een abonnement is gemaakt, dat ervoor zorgen dat er een of meer abonnementen onder het onderwerp |
| Onderwerp beleidsnaam | Wanneer u een onderwerp maakt, kunt u ook gedeelde-beleid maken op het tabblad onderwerp configureren. Elk gedeelde-beleid wordt een naam hebben, machtigingen die u hebt ingesteld, en de toegangstoetsen |
| Onderwerp beleid-toets | De toegang tot gedeelde sleutel gebruikt om te verifiëren toegang tot de Service Bus naamruimte |
| Gebeurtenis serialisatie-indeling | Serialiseringsindeling voor uitvoergegevens.  JSON, CSV- en Avro worden ondersteund. |
| Codering | Als CSV- of JSON opmaken, moet een codering worden opgegeven. UTF-8 is de enige ondersteunde codering indeling op dit moment |
| Een scheidingsteken | Alleen van toepassing op CSV-serialisatie. Stream Analytics ondersteunt een aantal veelvoorkomende scheidingstekens voor serialiseren gegevens in een CSV-indeling. Ondersteunde waarden zijn komma, puntkomma, spatie, tab en verticale balk. |

## <a name="documentdb"></a>DocumentDB

[Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) is een volledig beheerde NoSQL document databaseservice biedt een query en transacties via schema gegevens overzichtelijk en betrouwbare prestaties en snelle ontwikkeling.

De onderstaande tabel bevat de eigenschapnamen en de bijbehorende beschrijvingen voor het maken van een DocumentDB-uitvoer.

<table>
<tbody>
<tr>
<td>NAAM VAN EIGENSCHAP</td>
<td>BESCHRIJVING</td>
</tr>
<tr>
<td>Accountnaam</td>
<td>De naam van het account DocumentDB.  Dit is ook het eindpunt voor het account.</td>
</tr>
<tr>
<td>Accountsleutel</td>
<td>De gedeelde toegangstoets voor het account DocumentDB.</td>
</tr>
<tr>
<td>Database</td>
<td>De naam van de DocumentDB-database.</td>
</tr>
<tr>
<td>De syntaxis van de siteverzameling</td>
<td>De mapnaam van de siteverzameling voor de verzamelingen moet worden gebruikt. De indeling van de siteverzameling worden gemaakt volgens de token optioneel {partition}, waar partities starten vanuit 0.<BR>Bijvoorbeeld De volgende elementen zijn geldige invoer:<BR>MyCollection {partition}<BR>MyCollection<BR>Houd er rekening mee dat verzamelingen moeten bestaan voordat de Stream Analytics-taak wordt gestart en wordt niet automatisch worden gemaakt.</td>
</tr>
<tr>
<td>Partition-toets</td>
<td>De naam van het veld in de uitvoer gebeurtenissen gebruikt de sleutel voor uitvoer partitioneren over verzamelingen te geven.</td>
</tr>
<tr>
<td>Document-ID</td>
<td>De naam van het veld in de uitvoer-gebeurtenissen die worden gebruikt om op te geven van de primaire sleutel waarin invoegen of bijwerken bewerkingen zijn gebaseerd op.</td>
</tr>
</tbody>
</table>


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
