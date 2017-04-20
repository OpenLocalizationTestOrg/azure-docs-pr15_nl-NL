<properties
    pageTitle="Opslag Analytics gebruiken voor het verzamelen van gegevens logboeken en aan de doelstellingen | Microsoft Azure"
    description="Opslag Analytics kunt u voor het bijhouden van gegevens aan de doelstellingen voor alle opslagservices en voor het verzamelen van Logboeken voor Blob, wachtrij en tabel opslag."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="storage-analytics"></a>Opslag Analytics

## <a name="overview"></a>Overzicht

Azure opslag Analytics voert logboekregistratie en gegevens van de doelstellingen voor een account opslagruimte biedt. U kunt deze gegevens aanvragen aanwijzen, gebruik trends analyseren en diagnose stellen bij problemen met uw account opslag.

Als u wilt gebruiken opslag analyses, moet u deze inschakelen afzonderlijk voor elke service die u wilt controleren. U kunt deze inschakelen in de [Portal van Azure](https://portal.azure.com). Zie [Monitor met een account opslag in de Portal Azure](storage-monitor-storage-account.md)voor meer informatie. Ook kunt u opslagruimte Analytics via een programma via de REST API of de clientbibliotheek inschakelen. De bewerkingen [Blob Service eigenschappen](https://msdn.microsoft.com/library/hh452239.aspx), [Wachtrij Service eigenschappen](https://msdn.microsoft.com/library/hh452243.aspx) [Tabeleigenschappen Service ophalen](https://msdn.microsoft.com/library/hh452238.aspx)en [Bestandseigenschappen Service ophalen](https://msdn.microsoft.com/library/mt427369.aspx) gebruiken voor opslag-analyses inschakelen voor elke service.

De samengevoegde gegevens wordt opgeslagen in een bekende blob (voor logboekregistratie) en in bekende tabellen (voor de doelstellingen), die kunnen worden geopend met de Blob-service en tabel service API's.

Opslag Analytics geldt een limiet 20 TB van de hoeveelheid opgeslagen gegevens die onafhankelijk is van de totale limiet voor uw account opslag. Zie voor meer informatie over de facturering en bewaarbeleid voor gegevens, [opslag analyses en facturering](https://msdn.microsoft.com/library/hh360997.aspx). Zie voor meer informatie over het account opslaglimieten [Azure opslag schaalbaarheid en prestaties doelen](storage-scalability-targets.md).

Zie voor een uitgebreide handleiding over het gebruik van opslagruimte analyses en andere hulpprogramma's voor identificeren en oplossen van problemen met Azure opslagruimte met een diagnose stellen bij [Monitor, automatisch opsporen, en problemen met Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).


## <a name="about-storage-analytics-logging"></a>Informatie over logboekregistratie voor opslag-analyses

Opslag Analytics registreert gedetailleerde informatie over geslaagde en mislukte aanvragen met een storage-service. Deze informatie kan worden gebruikt om de afzonderlijke aanvragen te houden en problemen met een storage-service op te sporen. Aanvragen worden op basis van de efficiënt mogelijk geregistreerd.

Logboekvermeldingen worden gemaakt alleen als er opslagruimte serviceactiviteit. Bijvoorbeeld als een opslag-account activiteit in de service Blob maar niet in de tabel of wachtrij services heeft, wordt alleen logboeken die betrekking hebben op de service Blob gemaakt.

Opslag Analytics logboekregistratie is niet beschikbaar voor de Service Azure-bestand.

### <a name="logging-authenticated-requests"></a>Geverifieerd logboekregistratieaanvragen

De volgende soorten geverifieerde aanvragen zijn geregistreerd:

- Voltooide aanvragen.

- Mislukte aanvragen, inclusief time-out, beperken, netwerk, autorisatie en andere fouten.

- Aanvragen via van een gedeeld Access handtekening (SA's), met inbegrip van mislukte en geslaagd aanvragen.

- Aanvragen voor analytics-gegevens.

Aanvragen die zijn aangebracht door opslag Analytics zelf, zoals een logboek maken of verwijderen, worden niet geregistreerd. Een volledige lijst met gegevens in het logboek wordt beschreven in de [opslagruimte Analytics geregistreerde bewerkingen en statusberichten](https://msdn.microsoft.com/library/hh343260.aspx) en [Opslagindeling Analytics Log](https://msdn.microsoft.com/library/hh343259.aspx) onderwerpen.

### <a name="logging-anonymous-requests"></a>Logboekregistratie anonieme aanvragen
De volgende soorten anonieme aanvragen worden geregistreerd:

- Voltooide aanvragen.

- Serverfouten.

- Time-out fouten voor zowel clients en servers.

- Mislukte GET aanvragen met foutcode 304 (niet gewijzigd).

Alle andere mislukte anonieme aanvragen bent niet aangemeld. Een volledige lijst met gegevens in het logboek wordt beschreven in de [opslagruimte Analytics geregistreerde bewerkingen en statusberichten](https://msdn.microsoft.com/library/hh343260.aspx) en [Opslagindeling Analytics Log](https://msdn.microsoft.com/library/hh343259.aspx) onderwerpen.

### <a name="how-logs-are-stored"></a>Hoe logboeken worden opgeslagen
Alle logboeken worden opgeslagen in blok BLOB's in een container met de naam $logs, die automatisch wordt gemaakt wanneer opslag Analytics is ingeschakeld voor een opslag-account. De container $logs bevindt zich in de naamruimte blob van de opslag-account, bijvoorbeeld: `http://<accountname>.blob.core.windows.net/$logs`. Deze container kan niet worden verwijderd nadat opslag Analytics is ingeschakeld, hoewel de inhoud ervan kunnen worden verwijderd.

>[Azure.NOTE] De container $logs niet wordt weergegeven wanneer een container vermelding bewerking wordt uitgevoerd, zoals de methode [ListContainers](https://msdn.microsoft.com/library/azure/dd179352.aspx) . Dit moet rechtstreeks worden geopend. U kunt bijvoorbeeld de [ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx) -methode gebruiken voor toegang tot de BLOB's in de `$logs` container.
Zoals aanvragen bent aangemeld, wordt de opslag Analytics tussenliggende resultaten als blokken uploaden. Opslag-analyses wordt geregeld, deze blokken doorvoeren en zodat ze beschikbaar als een blob.

Dubbele records kunnen bestaan voor logboeken die is gemaakt in hetzelfde uur. Als een record een duplicaat is door te schakelen van de **aanvraag-id** en **bewerking** getal, kunt u bepalen.

### <a name="log-naming-conventions"></a>Meld u naamgevingsconventies
Elke log worden in de volgende indeling geschreven.

    <service-name>/YYYY/MM/DD/hhmm/<counter>.log

De volgende tabel worden alle kenmerken in de naam van het logboek.

| Kenmerk         | Beschrijving                                                                                                                                                                                   |
|----------------   |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------   |
| < Servicenaam >    | De naam van de storage-service. Bijvoorbeeld: blob, tabel of wachtrij.                                                                                                                          |
| JJJJ              | Het jaar van vier cijfers voor het logboek. Bijvoorbeeld: 2011.                                                                                                                                           |
| MM                | De maand in twee cijfers voor het logboek. Bijvoorbeeld: 07.                                                                                                                                             |
| DD                | De maand in twee cijfers voor het logboek. Bijvoorbeeld: 07.                                                                                                                                             |
| [hh                | Het uur in twee cijfers die het eerste uur voor de logboeken in 24-uursnotatie UTC aangeeft. Bijvoorbeeld: 18.                                                                                     |
| mm                | Het getal van twee cijfers die de beginwaarden minuut voor de logboeken aangeeft. Deze waarde wordt niet ondersteund in de huidige versie van opslag analyses, en wordt de waarde altijd 00.     |
| <counter>         | Een nul item met zes cijfers die het aantal log BLOB's die zijn gegenereerd voor de storage-service in een uur periode aangeeft. Dit item begint op 000000. Bijvoorbeeld: 000001.     |

Hierna volgt een naam voor het logboek van voltooid voorbeeld waarin de voorgaande voorbeelden zijn gecombineerd.

    blob/2011/07/31/1800/000001.log

Hierna volgt een voorbeeld URI die kan worden gebruikt voor toegang tot de vorige logboek.

    https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log

Wanneer een aanvraag opslag is aangemeld, wordt de resulterende aanmeldingsnaam betrekking heeft op het uur wanneer de bewerking is voltooid. Als u een verzoek voor een GetBlob is voltooid om 6:30 uur bijvoorbeeld op 7/31/2011, zou het logboek worden opgeslagen met de volgende prefix:`blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Meld u metagegevens
Alle log BLOB's zijn opgeslagen met metagegevens die kan worden gebruikt om aan te geven welke logboekgegevens de label bevat. De volgende tabel wordt elke metagegevenskenmerk.

| Kenmerk     | Beschrijving                                                                                                                                                                                                                                                   |
|------------   |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| LogType       | Beschrijving of het logboek informatie die betrekking hebben op als u wilt lezen bevat, schrijven of te verwijderen. Deze waarde kan één type of een combinatie van alle drie, gescheiden door komma's bevatten. Voorbeeld 1: schrijven; Voorbeeld 2: lezen, schrijven; Voorbeeld 3: lezen, schrijven, verwijderen.    |
| Starttijd     | De eerste keer van een vermelding in het logboek, in de vorm van jjjj-MM-ddTHH. Bijvoorbeeld: 2011-07-31T18:21:46Z.                                                                                                                                             |
| Eindtijd       | De meest recente tijd van een vermelding in het logboek, in de vorm van jjjj-MM-ddTHH. Bijvoorbeeld: 2011-07-31T18:22:09Z.                                                                                                                                               |
| LogVersion    | De versie van de indeling. De enige ondersteunde waarde is momenteel 1.0.                                                                                                                                                                                     |

De volgende lijst wordt weergegeven met de voorgaande Column voltooid voorbeeld-metagegevens.

- LogType = schrijven

- Starttijd = 2011-07-31T18:21:46Z

- Eindtijd = 2011-07-31T18:22:09Z

- LogVersion = 1,0

### <a name="accessing-logging-data"></a>Toegang krijgen tot het logboekgegevens

Alle gegevens in de `$logs` container kunnen worden geopend met behulp van de Blob-service API's, inclusief de .NET-API's gekregen van de Azure beheerde bibliotheek. De accountbeheerder opslag kunt lezen en zich aanmeldt, verwijderen, maar niet maken of bijgewerkt om ze. Zowel de metagegevens van het logboek als de naam van het logboek kunnen worden gebruikt bij het opvragen van een logboek. Is het mogelijk dat de logboekbestanden van een bepaalde tijd moet worden weergegeven volgorde, maar de metagegevens van altijd Hiermee geeft u het interval van logboekvermeldingen in een logboek. U kunt een combinatie van het met logboeknamen en metagegevens daarom bij het zoeken naar een bepaald logboek.

## <a name="about-storage-analytics-metrics"></a>Over opslag Analytics aan de doelstellingen

Opslag Analytics kunt aan de doelstellingen met geaggregeerde transactie statistieken en capaciteit gegevens over aanvragen voor een opslagservice opslaan. Transacties zijn gerapporteerd zowel het API bewerking niveau en op het niveau van de service opslag en capaciteit is gerapporteerd op het niveau van de service opslag. Aan de doelstellingen gegevens kunnen worden gebruikt als u wilt analyseren servicegebruik opslag, diagnose stellen bij problemen met aanvragen ten opzichte van de storage-service, en te verbeteren de prestaties van toepassingen die gebruikmaken van een service.

Als u wilt gebruiken opslag analyses, moet u deze inschakelen afzonderlijk voor elke service die u wilt controleren. U kunt deze inschakelen in de [Portal van Azure](https://portal.azure.com). Zie [Monitor met een account opslag in de Portal Azure](storage-monitor-storage-account.md)voor meer informatie. Ook kunt u opslagruimte Analytics via een programma via de REST API of de clientbibliotheek inschakelen. De bewerkingen **Service eigenschappen** gebruiken voor het inschakelen van opslag Analytics voor elke service.

### <a name="transaction-metrics"></a>Transactie aan de doelstellingen

Een reeks robuuste gegevens per uur of minuut interval voor elke storage-service is opgenomen en API-bewerking, inclusief ingress/egress, beschikbaarheid, fouten, aangevraagd en verzoek percentages gecategoriseerd. U kunt een volledige lijst van de transactiedetails in het onderwerp [Opslag Analytics aan de doelstellingen tabelschema](https://msdn.microsoft.com/library/hh343264.aspx) ziet.

Transactiegegevens worden vastgelegd op twee niveaus – het serviceniveau van de en het niveau van de API-bewerking. Op het serviceniveau van de, statistieken samenvatten alle aangevraagd API bewerkingen worden geschreven naar een Tabelentiteit per uur zelfs als er geen aanvragen zijn aangebracht in de service. Op het niveau van de bewerking API statistieken alleen naar een entiteit geschreven als de bewerking is aangevraagd binnen dat uur.

Bijvoorbeeld als u een bewerking **GetBlob** van uw service Blob uitvoert, Analytics-metrische opslaggegevens wordt Meld u aan de aanvraag en deze opnemen in de samengevoegde gegevens voor zowel de Blob-service, evenals de bewerking **GetBlob** . Echter als er geen bewerking **GetBlob** wordt aangevraagd tijdens het uur, een entiteit niet geschreven naar de `$MetricsTransactionsBlob` voor de bewerking.

Transactie aan de doelstellingen worden voor gebruikersaanvragen en aanvragen die zijn aangebracht door opslag-analyses zelf opgenomen. Bijvoorbeeld worden aanvragen door opslag Analytics voor het schrijven van Logboeken en tabel entiteiten opgenomen. Zie voor meer informatie over hoe deze aanvragen zijn gefactureerd, [opslag analyses en facturering](https://msdn.microsoft.com/library/hh360997.aspx).

### <a name="capacity-metrics"></a>De doelstellingen van de capaciteit

>[AZURE.NOTE] Capaciteit aan de doelstellingen zijn momenteel alleen beschikbaar voor de service Blob. Capaciteit van de doelstellingen voor de tabel service en wachtrijservice zijn beschikbaar in toekomstige versies van opslag Analytics.

Capaciteit gegevens dagelijks is vastgelegd voor een opslag-account Blob service en twee tabel entiteiten zijn geschreven. Één entiteit bevat statistieken voor gebruikersgegevens en de andere bevat statistieken over de `$logs` blob container die wordt gebruikt door opslag Analytics. De `$MetricsCapacityBlob` tabel bevat de volgende statistieken:

- **Capaciteit**: de hoeveelheid opslagruimte die wordt gebruikt door de opslag van de account Blob-service, in bytes.

- **ContainerCount**: het aantal blob containers in Blob-service van de opslag-account.

- **ObjectCount**: het aantal vastgelegde en niet-doorgevoerde blokkeren of de pagina BLOB's in de opslag van de account Blob service.

Zie voor meer informatie over de doelstellingen van de capaciteit, [Opslag Analytics aan de doelstellingen tabelschema](https://msdn.microsoft.com/library/hh343264.aspx).

### <a name="how-metrics-are-stored"></a>Hoe de doelstellingen worden opgeslagen

Alle gegevens van de doelstellingen voor elk van de opslagservices is opgeslagen in de drie tabellen gereserveerd voor deze service: één tabel voor transactie informatie, één tabel voor minuut transactie-informatie en capaciteit informatie in een andere tabel. Transactie en minuut transactie-informatie bestaat uit vergaderverzoeken en antwoorden gegevens en informatie over capaciteit bestaat uit gebruiksgegevens opslag. Aan de doelstellingen uur, minuut maatstaven en capaciteit van een opslag-account Blob service kunnen worden geopend in tabellen die zijn benoemde zoals is beschreven in de volgende tabel.

| Aan de doelstellingen niveau                         | Tabelnamen                                                                                                                   | Ondersteunde versies                                                                                                                        |
|------------------------------------   |-----------------------------------------------------------------------------------------------------------------------------  |---------------------------------------------------------------------------------------------------------------------------------------------- |
| Ieder uur aan de doelstellingen, primaire locatie      |  $MetricsTransactionsBlob <br/>$MetricsTransactionsTable <br/> $MetricsTransactionsQueue                                                  | Versies vóór 2013-08-15 alleen. Hoewel deze namen zijn nog steeds wordt ondersteund, wordt het wordt aanbevolen dat u overstappen op een van de onderstaande tabellen.  |
| Ieder uur aan de doelstellingen, primaire locatie      | $MetricsHourPrimaryTransactionsBlob <br/>$MetricsHourPrimaryTransactionsTable <br/>$MetricsHourPrimaryTransactionsQueue               | Alle versies, inclusief 2013-08-15.                                                                                                               |
| Minuut doelstellingen, primaire locatie      | $MetricsMinutePrimaryTransactionsBlob <br/>$MetricsMinutePrimaryTransactionsTable <br/>$MetricsMinutePrimaryTransactionsQueue         | Alle versies, inclusief 2013-08-15.                                                                                                               |
| Ieder uur aan de doelstellingen, secundaire locatie    | $MetricsHourSecondaryTransactionsBlob <br/>$MetricsHourSecondaryTransactionsTable <br/>$MetricsHourSecondaryTransactionsQueue         | Alle versies, inclusief 2013-08-15. Leestoegang geografische-redundante herhaling moet zijn ingeschakeld.                                                    |
| Minuut doelstellingen, secundaire locatie    | $MetricsMinuteSecondaryTransactionsBlob <br/>$MetricsMinuteSecondaryTransactionsTable <br/>$MetricsMinuteSecondaryTransactionsQueue   | Alle versies, inclusief 2013-08-15. Leestoegang geografische-redundante herhaling moet zijn ingeschakeld.                                                    |
| Capaciteit (alleen Blob-service)          | $MetricsCapacityBlob                                                                                                          | Alle versies, inclusief 2013-08-15.                                                                                                               |


Deze tabellen worden automatisch gemaakt wanneer opslag Analytics is ingeschakeld voor een opslag-account. Ze toegankelijk zijn via de naamruimte van de opslag-account, bijvoorbeeld:`https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`

### <a name="accessing-metrics-data"></a>Toegang tot gegevens aan de doelstellingen

Alle gegevens in de tabellen aan de doelstellingen kunnen worden geopend met behulp van de tabel service API's, inclusief de .NET-API's gekregen van de Azure beheerde bibliotheek. De accountbeheerder opslag kunt lezen en verwijderen van de tabel entiteiten, maar niet maken of bijgewerkt om ze.

## <a name="billing-for-storage-analytics"></a>Facturering voor opslag-analyses

Opslag Analytics is ingeschakeld door een accounteigenaar opslag; het is niet standaard ingeschakeld. Alle gegevens van de doelstellingen is geschreven door de services van een opslag-account. Elke bewerking uitgevoerd door de opslagruimte Analytics is daardoor factureerbare. Bovendien is de hoeveelheid opslagruimte die wordt gebruikt door de doelstellingen gegevens ook factureerbare.

De volgende acties uitgevoerd door de opslagruimte Analytics zijn factureerbare:

- Aanvragen voor het maken van BLOB's voor logboekregistratie. 

- Aanvragen voor het maken van de tabel entiteiten aan de doelstellingen.

Als u een bewaarbeleid voor gegevens hebt geconfigureerd, u zijn geen btw geheven voor verwijdertransacties wanneer opslag Analytics worden de oude logboekregistratie en statistieken gegevens verwijderd. Er zijn echter verwijdertransacties van een client factureerbare. Zie [een bewaarbeleid opslag Analytics gegevens instellen](https://msdn.microsoft.com/library/azure/hh343263.aspx)voor meer informatie over het bewaarbeleid.

### <a name="understanding-billable-requests"></a>Lidmaatschap factureerbare aanvragen

Elk verzoek met een account storage-service is factureerbare of niet-factureerbare. Opslag Analytics logboeken elke afzonderlijke aanvraag aangebracht in een service, inclusief een statusbericht waarmee wordt aangegeven hoe de aanvraag is verwerkt. Opslag Analytics opgeslagen op dezelfde manier aan de doelstellingen voor zowel een service en de API-bewerkingen van die service, inclusief de percentages en telling van bepaalde statusberichten. Samen en kunt deze functies u uw factureerbare aanvragen analyseren, verbeteren op uw toepassing en een diagnose stellen bij problemen met aanvragen voor uw services. Zie [Wat Azure opslag facturering - bandbreedte, transacties, en capaciteit](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx)voor meer informatie over facturering.

Wanneer u opslagruimte Analytics gegevens bekijkt, kunt u de tabellen in het onderwerp [opslag Analytics geregistreerde bewerkingen en statusberichten](https://msdn.microsoft.com/library/azure/hh343260.aspx) om te bepalen welke aanvragen worden factureerbare. Vervolgens kunt u uw logboeken en aan de doelstellingen-gegevens op de van statusberichten om te zien als u in de rekening zijn gebracht voor een bepaald verzoek vergelijken. U kunt ook de tabellen in het vorige onderwerp gebruiken om te kunnen onderzoeken beschikbaarheid voor een storage-service of de afzonderlijke API-bewerking.

## <a name="next-steps"></a>Volgende stappen

### <a name="setting-up-storage-analytics"></a>Instellen opslag Analytics
- [Monitor met een account opslag in de Portal van Azure](storage-monitor-storage-account.md)
- [Inschakelen en configureren van opslag Analytics](https://msdn.microsoft.com/library/hh360996.aspx)

### <a name="storage-analytics-logging"></a>Opslag Analytics logboekregistratie  
- [Informatie over logboekregistratie voor opslag-analyses](https://msdn.microsoft.com/library/hh343262.aspx)
- [Opslagindeling Analytics Log](https://msdn.microsoft.com/library/hh343259.aspx)
- [Opslag Analytics vastgelegd bewerkingen en statusberichten](https://msdn.microsoft.com/library/hh343260.aspx)

### <a name="storage-analytics-metrics"></a>Opslageenheden Analytics
- [Over opslageenheden Analytics](https://msdn.microsoft.com/library/hh343258.aspx)
- [Opslag Analytics aan de doelstellingen tabelschema](https://msdn.microsoft.com/library/hh343264.aspx)
- [Opslag Analytics vastgelegd bewerkingen en statusberichten](https://msdn.microsoft.com/library/hh343260.aspx)  
