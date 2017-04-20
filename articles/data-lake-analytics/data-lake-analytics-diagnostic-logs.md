<properties 
   pageTitle="Diagnostische logboeken bekijken voor Azure gegevens Lake Analytics | Microsoft Azure" 
   description="Begrijpen hoe installeren en toegang tot de diagnostische logboeken van Azure gegevens Lake analysegegevens " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="Blackmist" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="08/11/2016"
   ms.author="larryfr"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>Diagnostische logboeken openen van Azure gegevens Lake analysegegevens

Meer informatie over het inschakelen van diagnostische gegevens vastleggen voor uw gegevens Lake Analytics-account en het weergeven van de logboekbestanden die worden verzameld voor uw account.

Organisaties kunnen diagnostische gegevens vastleggen voor hun rekening Azure gegevens Lake Analytics voor het verzamelen van gegevens access audit-registratie inschakelen. Deze logboeken bevatten informatie, zoals:

* Een lijst met gebruikers die de gegevens gebruikt.
* Hoe vaak de gegevens wordt geopend.
* Hoeveel gegevens worden opgeslagen in het account.

## <a name="prerequisites"></a>Vereisten voor

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
- **Uw abonnement op Azure inschakelen** voor openbare voorbeeld van gegevens Lake Analytics. Zie de [instructies](data-lake-analytics-get-started-portal.md#signup).
- **Azure gegevens Lake Analytics-account**. Volg de instructies bij [aan de slag met Azure gegevens Lake analyses met behulp van de Azure portal](data-lake-analytics-get-started-portal.md).

## <a name="enable-logging"></a>Logboekregistratie inschakelen

1. Aanmelden bij de nieuwe [Azure-portal](https://portal.azure.com).

2. Uw gegevens Lake Analytics-account, opent en uw gegevens Lake Analytics account blade, klik op **Instellingen**en klik vervolgens op **Diagnostische instellingen**.

3. Controleer in het **Diagnostische** blad, de volgende wijzigingen in de diagnostische gegevens vastleggen configureren.

    ![Diagnostische gegevens vastleggen] (./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "Diagnostische logboeken inschakelen")

    * **Aanwezigheidsstatus** instellen op **op** Diagnostische logboekregistratie inschakelen.
    * U kunt store/proces de gegevens op twee verschillende manieren.
        * Selecteer **exporteren naar gebeurtenis Hub** naar stream logboekgegevens op een Hub Azure-gebeurtenis. Gebruik deze optie als u een pijplijn volgende verwerking om te analyseren binnenkomende Logboeken in realtime hebt. Als u deze optie selecteert, moet u de details opgeven voor de Azure gebeurtenis Hub die u wilt gebruiken.
        * Selecteer **exporteren naar opslag-Account** om op te slaan Logboeken bij een Azure Storage-account. Gebruik deze optie als u wilt de gegevens archiveren. Als u deze optie selecteert, moet u een account Azure Storage om op te slaan de logboeken naar opgeven.
    * Opgeven of u wilt u controlelogboeken of verzoek Logboeken of beide.
    * Geef het aantal dagen waarvoor de gegevens moeten worden bewaard.
    * Klik op **Opslaan**.

Zodra u diagnostische instellingen hebt ingeschakeld, kunt u de logboeken op het tabblad **Diagnostische logboeken** kunt bekijken.

## <a name="view-logs"></a>Logboeken weergeven

Er zijn twee manieren om weer te geven van de logboekgegevens voor uw gegevens Lake Analytics-account.

* Via de gegevens Lake Analytics-Accountinstellingen
* Vanuit de opslag van Azure-account waar de gegevens worden opgeslagen

### <a name="using-the-data-lake-analytics-settings-view"></a>Gebruik de instellingen voor gegevens Lake Analytics-weergave

1. Van uw gegevens Lake Analytics account **Instellingen** blade, klikt u op **Diagnostische logboeken**.

    ![Weergave diagnostische gegevens vastleggen] (./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "Diagnostische logboeken weergeven") 

2. In het blad **Diagnostische logboeken** ziet u de logboeken met **Controlelogboeken bijhouden** en **Logboeken aanvragen**gecategoriseerd.
    * Aanvraag logboeken vastleggen elke API verzoek voor de gegevens Lake Analytics-account.
    * Controlelogboeken bijhouden zijn vergelijkbaar met Logboeken aanvragen maar om er zelf een veel gedetailleerd overzicht van de bewerkingen die wordt uitgevoerd op de gegevens Lake Analytics-account. Bijvoorbeeld een API-aanroep voor één upload in Logboeken aan de aanvraag kan leiden tot meerdere 'Toevoegen' bewerkingen in de controlelogboeken bijhouden.

3. Klik op de koppeling **downloaden** voor een logboek downloaden van de logboeken.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Vanuit de opslag van Azure-account waarvan de logboekgegevens

1. Open het Azure Storage account blad dat is gekoppeld aan gegevens Lake Analytics voor logboekregistratie en klik vervolgens op BLOB's. Het blad **Blob service** bevat twee notitiecontainers.

    ![Weergave diagnostische gegevens vastleggen] (./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "Diagnostische logboeken weergeven")

    * Als u de container **inzichten-logboeken-audit** bevat de controlelogboeken bijhouden.
    * De container **inzichten-logboeken-aanvragen** bevat de logboeken aan de aanvraag.

2. In deze containers, de logboeken opgeslagen onder de volgende structuur.

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json
    
    > [AZURE.NOTE] De `##` vermeldingen in het pad bevatten het jaar, maand, dag en uur waarin het logboek is gemaakt. Gegevens Lake Analytics Hiermee maakt u één bestand per uur, dus `m=` bevat altijd een waarde van `00`.

    Als u bijvoorbeeld het volledige pad naar een controlelogboek mogelijk:
    
        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    Daarnaast het volledige pad naar een logboek verzoek mogelijk:
    
        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a>Log structuur

De logboeken aan de controle en aanvraag zijn in een indeling van JSON. In deze sectie, kijkt u naar de structuur van JSON voor een aangevraagde we en controlelogboeken.

### <a name="request-logs"></a>Logboeken aanvragen

Hier ziet u de invoer in een voorbeeld in het logboek verzoek JSON-indeling. Elke blob heeft één hoofdmap object met de naam van de **records** die een matrix van log objecten bevat.

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>Log schema aanvragen

| Naam            | Type   | Beschrijving                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| tijd            | Tekenreeks | De tijdstempel (in UTC) van het logboek                                              |
| resourceId      | Tekenreeks | De ID van de resource die werd plaats op                            |
| categorie        | Tekenreeks | De categorie log. Bijvoorbeeld: **aanvragen**.                                   |
| operationName   | Tekenreeks | De naam van de bewerking die zijn vastgelegd. Bijvoorbeeld: GetAggregatedJobHistory.              |
| resultType      | Tekenreeks | De status van de bewerking, bijvoorbeeld 200.                                 |
| callerIpAddress | Tekenreeks | Het IP-adres van de client die de aanvraag                                |
| correlationId   | Tekenreeks | De id van het logboek. Deze waarde kan worden gebruikt om een aantal gerelateerde logboekvermeldingen groeperen |
| identiteit        | Object | De identiteit die het logboek gegenereerd                                            |
| Eigenschappen      | JSON   | Raadpleeg het volgende gedeelte (verzoek log eigenschappen schema) voor meer informatie |

#### <a name="request-log-properties-schema"></a>Log eigenschappen schema aanvragen

| Naam                 | Type   | Beschrijving                                               |
|----------------------|--------|-----------------------------------------------------------|
| HttpMethod           | Tekenreeks | De HTTP-methode gebruikt voor de bewerking. Bijvoorbeeld, krijgen. |
| Pad                 | Tekenreeks | Het pad naar de bewerking is uitgevoerd op                   |
| RequestContentLength | int    | De lengte van de inhoud van de HTTP-aanvraag                    |
| ClientRequestId      | Tekenreeks | De Id die uniek deze aanvraag              |
| Starttijd            | Tekenreeks | Het tijdstip waarop de server de aanvraag ontvangen         |
| Eindtijd              | Tekenreeks | Het tijdstip waarop de server een antwoord verzonden              |

### <a name="audit-logs"></a>Controlelogboeken bijhouden

Hier ziet u de invoer in een voorbeeld in het controlelogboek die door de JSON-indeling. Elke blob heeft één hoofdmap object met de naam van de **records** die een matrix van log objecten bevat

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job", 
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Audit log schema

| Naam            | Type   | Beschrijving                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| tijd            | Tekenreeks | De tijdstempel (in UTC) van het logboek                                              |
| resourceId      | Tekenreeks | De ID van de resource die werd plaats op                            |
| categorie        | Tekenreeks | De categorie log. Bijvoorbeeld: **controle**.                                      |
| operationName   | Tekenreeks | De naam van de bewerking die zijn vastgelegd. Bijvoorbeeld: JobSubmitted.              |
| resultType | Tekenreeks | Een substatus voor de taakstatus (operationName). |
| resultSignature | Tekenreeks | Meer informatie over de taakstatus (operationName). |
| identiteit      | Tekenreeks | De gebruiker die de bewerking aangevraagd. Bijvoorbeeld susan@contoso.com.                                 |
| Eigenschappen      | JSON   | Raadpleeg het volgende gedeelte (controle log eigenschappen schema) voor meer informatie |

> [AZURE.NOTE] __resultType__ __resultSignature__ vindt u informatie over het resultaat van een bewerking en alleen een waarde bevatten als een bewerking is voltooid. Bijvoorbeeld bevatten ze een waarde als __operationName__ een waarde van __JobStarted__ of __JobEnded bevat__.

#### <a name="audit-log-properties-schema"></a>Audit log eigenschappen schema

| Naam       | Type   | Beschrijving                              |
|------------|--------|------------------------------------------|
| Taak-id | Tekenreeks | De ID die is toegewezen aan het project  |
| Taaknaam | Tekenreeks | De naam die is opgegeven voor de taak |
| JobRunTime | Tekenreeks | De runtime gebruikt om de taak te verwerken |
| SubmitTime | Tekenreeks | De tijd (in UTC) waarop de taak is ingediend |
| Starttijd | Tekenreeks | De tijd dat de taak is gestart na indiening (in UTC). |
| Eindtijd | Tekenreeks | De tijd dat de taak is beëindigd. |
| Parallellisme | Tekenreeks | Het aantal aangevraagd voor deze taak tijdens het indienen van gegevens Lake Analytics-eenheden. |

> [AZURE.NOTE] __SubmitTime__, __Starttijd__, __eindtijd__ __parallellisme__ vindt u informatie over een bewerking en alleen een waarde bevatten als een bewerking is begonnen of voltooid. __SubmitTime__ bevat bijvoorbeeld een waarde nadat __operationName__ __JobSubmitted__wordt aangegeven.

## <a name="process-the-log-data"></a>De logboekgegevens van proces

Azure gegevens Lake Analytics biedt een steekproef aan hoe worden verwerkt en de logboekgegevens analyseren. U kunt de steekproef op [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample)vinden. 


## <a name="next-steps"></a>Volgende stappen

- [Overzicht van Azure gegevens Lake Analytics](data-lake-analytics-overview.md)


