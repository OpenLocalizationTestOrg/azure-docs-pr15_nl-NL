<properties 
   pageTitle="Diagnostische logboeken voor Azure Lake gegevensopslag bekijken | Microsoft Azure" 
   description="Begrijpen hoe installeren en toegang tot de diagnostische logboeken voor Azure Lake gegevensopslag " 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-store"></a>Toegang krijgen tot de diagnostische logboeken voor Azure Lake gegevensopslag

Meer informatie over het inschakelen van diagnostische gegevens vastleggen voor uw account voor gegevensopslag Lake en het weergeven van de logboekbestanden die worden verzameld voor uw account.

Organisaties diagnostische gegevens vastleggen voor hun rekening Azure Lake gegevensopslag voor het verzamelen van gegevens access audit-registratie vindt u toegangsgegevens voor de lijst met gebruikers toegang krijgen tot de gegevens, hoe vaak de gegevens wordt geopend, kunnen inschakelen hoeveel gegevens worden opgeslagen in het account, enzovoort.

## <a name="prerequisites"></a>Vereisten voor

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

- **De gegevensopslag Lake azure-account**. Volg de instructies bij [aan de slag met Azure Lake gegevensopslag met behulp van de Azure-Portal](data-lake-store-get-started-portal.md).

## <a name="enable-diagnostic-logging-for-your-data-lake-store-account"></a>Diagnostische gegevens vastleggen voor uw account voor gegevensopslag Lake inschakelen

1. Aanmelden bij de nieuwe [Azure-Portal](https://portal.azure.com).

2. Uw account voor gegevensopslag Lake, openen en uw account Lake gegevensopslag blade, klik op **Instellingen**en klik op **Diagnostische instellingen**.

3. Controleer in het **Diagnostische** blad, de volgende wijzigingen in de diagnostische gegevens vastleggen configureren.

    ![Diagnostische gegevens vastleggen] (./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "Diagnostische logboeken inschakelen")

    * **Aanwezigheidsstatus** instellen op **op** Diagnostische logboekregistratie inschakelen.
    * U kunt store/proces de gegevens op twee verschillende manieren.
        * Selecteer de optie exporteren **naar gebeurtenis Hub** naar stream logboekgegevens op een Hub Azure-gebeurtenis. Waarschijnlijk wordt gebruik deze optie als u een pijplijn volgende verwerking om te analyseren binnenkomende logboeken op realtime hebt. Als u deze optie selecteert, moet u de details opgeven voor de Azure gebeurtenis Hub die u wilt gebruiken.
        * Selecteer de optie exporteren **naar opslag-Account** om op te slaan Logboeken bij een Azure Storage-account. U gebruik deze optie als u wilt archiveren van de gegevens die batch verwerkt op een later tijdstip worden. Als u deze optie selecteert, moet u een Azure Storage-account om op te slaan de logboeken naar opgeven.
    * Opgeven of u wilt u controlelogboeken of verzoek Logboeken of beide.
    * Geef het aantal dagen waarvan de gegevens moeten worden bewaard.
    * Klik op **Opslaan**.

Zodra u diagnostische instellingen hebt ingeschakeld, kunt u de logboeken op het tabblad **Diagnostische logboeken** kunt bekijken.

## <a name="view-diagnostic-logs-for-your-data-lake-store-account"></a>Diagnostische logboeken weergeven voor uw account voor gegevensopslag Lake

Er zijn twee manieren om weer te geven van de logboekgegevens voor uw account voor gegevensopslag Lake.

* In het account voor gegevensopslag Lake weergeven instellingen
* Vanuit de opslag van Azure-account waar de gegevens worden opgeslagen

### <a name="using-the-data-lake-store-settings-view"></a>De weergave van de gegevens Lake Store instellingen gebruiken

1. Klik op uw Lake gegevensopslag account **Instellingen** blade, op **Diagnostische logboeken**.

    ![Weergave diagnostische gegevens vastleggen] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "Diagnostische logboeken weergeven") 

2. In het blad **Diagnostische logboeken** ziet u de logboeken met **Controlelogboeken bijhouden** en **Logboeken aanvragen**gecategoriseerd.
    * Aanvraag logboeken vastleggen elk verzoek API op het account voor gegevensopslag Lake.
    * Controlelogboeken bijhouden zijn vergelijkbaar met Logboeken aanvragen maar om er zelf een veel gedetailleerd overzicht van de bewerkingen die wordt uitgevoerd op het account voor gegevensopslag Lake. Bijvoorbeeld een API-aanroep voor één upload in Logboeken aan de aanvraag kan leiden tot meerdere 'Toevoegen' bewerkingen in de controlelogboeken bijhouden.

3. Klik op de koppeling **downloaden** ten opzichte van elk item log downloaden van de logboeken.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Vanuit de opslag van Azure-account waarvan de logboekgegevens

1. Open het Azure Storage account blad dat is gekoppeld aan Lake gegevensopslag voor logboekregistratie en klik vervolgens op BLOB's. Het blad **Blob service** bevat twee notitiecontainers.

    ![Weergave diagnostische gegevens vastleggen] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "Diagnostische logboeken weergeven")

    * Als u de container **inzichten-logboeken-audit** bevat de controlelogboeken bijhouden.
    * De container **inzichten-logboeken-aanvragen** bevat de logboeken aan de aanvraag.

2. In deze containers, de logboeken opgeslagen onder de volgende structuur.

    ![Weergave diagnostische gegevens vastleggen] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "Diagnostische logboeken weergeven")

    Als u bijvoorbeeld kan het volledige pad naar een controlelogboek worden`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`

    Similary, het volledige pad naar een logboek verzoek kan worden`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## <a name="understand-the-structure-of-the-log-data"></a>Meer informatie over de structuur van de logboekgegevens

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
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Requests",
             "operationName": "GETCustomerIngressEgress",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
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
| operationName   | Tekenreeks | De naam van de bewerking die zijn vastgelegd. Bijvoorbeeld: getfilestatus.              |
| resultType      | Tekenreeks | De status van de bewerking, bijvoorbeeld 200.                                 |
| callerIpAddress | Tekenreeks | Het IP-adres van de client die de aanvraag                                |
| correlationId   | Tekenreeks | De id van het logboek die kan worden gebruikt om te groeperen van een reeks gerelateerde logboekvermeldingen |
| identiteit        | Object | De identiteit die het logboek gegenereerd                                            |
| Eigenschappen      | JSON   | Zie hieronder voor meer informatie                                                          |

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
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_store_account_name>.azuredatalakestore.net/logs.csv"}
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
| operationName   | Tekenreeks | De naam van de bewerking die zijn vastgelegd. Bijvoorbeeld: getfilestatus.              |
| resultType      | Tekenreeks | De status van de bewerking, bijvoorbeeld 200.                                 |
| correlationId   | Tekenreeks | De id van het logboek die kan worden gebruikt om te groeperen van een reeks gerelateerde logboekvermeldingen |
| identiteit        | Object | De identiteit die het logboek gegenereerd                                            |
| Eigenschappen      | JSON   | Zie hieronder voor meer informatie                                                          |

#### <a name="audit-log-properties-schema"></a>Audit log eigenschappen schema

| Naam       | Type   | Beschrijving                              |
|------------|--------|------------------------------------------|
| StreamName | Tekenreeks | Het pad naar de bewerking is uitgevoerd op  |


## <a name="samples-to-process-the-log-data"></a>Voorbeelden verwerkingstijd van de logboekgegevens

Azure Lake gegevensopslag biedt een steekproef aan hoe worden verwerkt en de logboekgegevens analyseren. U kunt de steekproef op [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample)vinden. 


## <a name="see-also"></a>Zie ook

- [Overzicht van Azure Lake gegevensopslag](data-lake-store-overview.md)
- [Beveiliging van gegevens in Lake gegevensopslag](data-lake-store-secure-data.md)

