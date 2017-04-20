<properties 
   pageTitle="Aan de slag met gegevens Lake analyses met REST API | Microsoft Azure" 
   description="WebHDFS REST API's gebruiken om u te bewerkingen uitvoeren op gegevens Lake Analytics" 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a>Aan de slag met Azure gegevens Lake analyses met REST API 's

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Informatie over het gebruik van WebHDFS REST API's en gegevens Lake Analytics REST API's voor het beheren van gegevens Lake Analytics-accounts, taken en -catalogus. 

## <a name="prerequisites"></a>Vereisten voor

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

- **Een Azure Active Directory-toepassing maken**. De Azure AD-toepassing kunt u de gegevens Lake Analytics-servicetoepassing met Azure AD verifiëren. Er zijn verschillende manieren om te verifiëren met Azure AD, welke **eindgebruikers** of **service-naar-service-verificatie**. Zie voor instructies en meer informatie over het om te verifiëren, [verificatie met gegevens Lake analytische mogelijkheden op basis van Azure Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).

- [omslaan](http://curl.haxx.se/). In dit artikel wordt krul gebruikt om te laten zien hoe u REST API oproepen tegen een gegevens Lake Analytics-account.

## <a name="authenticate-with-azure-active-directory"></a>Verificatie met Azure Active Directory

Er zijn twee methoden voor het verifiëren met Azure Active Directory.

### <a name="end-user-authentication-interactive"></a>Eindgebruikers verificatie (interactieve)

Deze methode wordt gevraagd of de gebruiker aan te melden en alle bewerkingen in de context van de gebruiker worden uitgevoerd. 

Volg deze stappen voor interactieve verificatie:

1. Via de toepassing, door de gebruiker te omleiden naar de volgende URL:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<REDIRECT-URI > moet worden gecodeerd voor gebruik in een URL. Ja, voor https://localhost, gebruikt u `https%3A%2F%2Flocalhost`)

    Voor de toepassing van deze zelfstudie kunt u de tijdelijke aanduiding voor waarden in de URL hierboven vervangen en plak deze in de adresbalk van de webbrowser. U wordt omgeleid als u wilt verifiëren met behulp van uw Azure login. Wanneer u met succes zich hebt aangemeld, wordt het antwoord wordt weergegeven in de adresbalk van de browser. Het antwoord niet in de volgende indeling:
        
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. De autorisatiecode uit het antwoord vastleggen. Voor deze zelfstudie kunt u de autorisatie gekopieerd van de adresbalk van de webbrowser en doorgegeven in de POST-aanvraag aan het token eindpunt, zoals hieronder wordt weergegeven:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>

    >[AZURE.NOTE] In dit geval de \<OMLEIDINGS-URI > niet moet worden gecodeerd.

3. Het antwoord is een JSON-object dat een toegangstoken bevat (bijvoorbeeld `"access_token": "<ACCESS_TOKEN>"`) en een token vernieuwen (bijvoorbeeld `"refresh_token": "<REFRESH_TOKEN>"`). Uw toepassing wordt bij het openen van Azure Lake gegevensopslag en het vernieuwen token het toegangstoken gebruikt om een andere toegangstoken wanneer een toegangstoken verloopt.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before": "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4.  Als het toegangstoken verloopt, kunt u een nieuwe toegangstoken met het token vernieuwen aanvragen, zoals hieronder wordt weergegeven:

         curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
            -F grant_type=refresh_token \
            -F resource=https://management.core.windows.net/ \
            -F client_id=<CLIENT-ID> \
            -F refresh_token=<REFRESH-TOKEN>
 
Zie voor meer informatie over interactieve gebruikersverificatie [autorisatiecode stroom verlenen](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Service-naar-service-verificatie (niet-interactieve)

Deze methode biedt toepassing een eigen referenties om de bewerkingen uitvoeren. Hiervoor moet u een POST-aanvraag zoals wordt weergegeven onder probleem: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

De uitvoer van deze aanvraag bevat een Autorisatietoken (aangeduid met `access-token` in de onderstaande uitvoer) die u vervolgens met uw oproepen REST API wordt doorgegeven. Deze verificatietoken opslaan in een tekstbestand; Dit moet u verderop in dit artikel.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

In dit artikel worden gebruikt de **niet-interactieve** methode. Zie voor meer informatie over niet-interactieve (service-naar-service oproepen), [Service aan service gesprekken met referenties](https://msdn.microsoft.com/library/azure/dn645543.aspx).
## <a name="create-a-data-lake-analytics-account"></a>Een gegevens Lake Analytics-account maken

U moet een Azure resourcegroep en een account voor gegevensopslag Lake maken voordat u een gegevens Lake Analytics-account kunt maken.  Zie [een account voor gegevensopslag Lake maken](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).

De volgende opdracht uit krul ziet u hoe u een account kunt maken:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

Vervang \< `REDACTED` \> met het Autorisatietoken \< `AzureSubscriptionID` \> met uw abonnements-ID, \< `AzureResourceGroupName` \> met de naam van een bestaande resourcegroep Azure, en \< `NewAzureDataLakeAnalyticsAccountName` \> met een nieuwe naam voor de gegevens Lake Analytics-Account. De aanvraag nettolading voor deze opdracht bevindt zich in het **CreateDatalakeAnalyticsAccountRequest.json** -bestand dat is opgegeven voor de `-d` parameter hierboven. De inhoud van het bestand input.json er dan ongeveer als volgt te werk:

    {  
        "location": "East US 2",  
        "name": "myadla1004",  
        "tags": {},  
        "properties": {  
            "defaultDataLakeStoreAccount": "my1004store",  
            "dataLakeStoreAccounts": [  
                {  
                    "name": "my1004store"  
                }     
            ]
        }  
    }  


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a>Lijst met gegevens Lake Analytics-accounts in een abonnement

De volgende opdracht uit krul ziet u hoe u accounts niet in een abonnement:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

Vervang \< `REDACTED` \> met het Autorisatietoken \< `AzureSubscriptionID` \> met uw abonnement-ID. De uitvoer is vergelijkbaar met:

    {
        "value": [
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla0831.azuredatalakeanalytics.net",
                "accountId": "21e74660-0941-4880-ae72-b143c2615ea9",
                "creationTime": "2016-09-01T12:49:12.7451428Z",
                "lastModifiedTime": "2016-09-01T12:49:12.7451428Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla0831rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla0831",
            "name": "myadla0831",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            },
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla1004.azuredatalakeanalytics.net",
                "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
                "creationTime": "2016-10-04T20:46:42.287147Z",
                "lastModifiedTime": "2016-10-04T20:46:42.287147Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
            "name": "myadla1004",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            }
        ]
    }

## <a name="get-information-about-a-data-lake-analytics-account"></a>Informatie over een gegevens Lake Analytics-account

De volgende opdracht uit krul ziet u hoe u de gegevens van een account:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

Vervang \< `REDACTED` \> met het Autorisatietoken \< `AzureSubscriptionID` \> met uw abonnements-ID, \< `AzureResourceGroupName` \> met de naam van een bestaande resourcegroep Azure, en \< `DataLakeAnalyticsAccountName` \> met de naam van een bestaande gegevens Lake Analytics-Account. De uitvoer is vergelijkbaar met:

    {
        "properties": {
            "defaultDataLakeStoreAccount": "my1004store",
            "dataLakeStoreAccounts": [
            {
                "properties": {
                "suffix": "azuredatalakestore.net"
                },
                "name": "my1004store"
            }
            ],
            "provisioningState": "Creating",
            "state": null,
            "endpoint": null,
            "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
            "creationTime": null,
            "lastModifiedTime": null
        },
        "location": "East US 2",
        "tags": {},
        "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
        "name": "myadla1004",
        "type": "Microsoft.DataLakeAnalytics/accounts"
    }

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a>Lijst met gegevens Lake winkels van een gegevens Lake Analytics-account

De volgende opdracht uit krul ziet u hoe u gegevens Lake winkels van een account:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

Vervang \< `REDACTED` \> met het Autorisatietoken \< `AzureSubscriptionID` \> met uw abonnements-ID, \< `AzureResourceGroupName` \> met de naam van een bestaande resourcegroep Azure, en \< `DataLakeAnalyticsAccountName` \> met de naam van een bestaande gegevens Lake Analytics-Account. De uitvoer is vergelijkbaar met:

    {
        "value": [
            {
            "properties": {
                "suffix": "azuredatalakestore.net"
            },
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004/dataLakeStoreAccounts/my1004store",
            "name": "my1004store",
            "type": "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts"
            }
        ]
    }

## <a name="submit-u-sql-jobs"></a>I-SQL-taken indienen

De volgende opdracht uit krul ziet hoe u een taak I-SQL verzenden:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

Vervang \< `REDACTED` \> met het Autorisatietoken \< `DataLakeAnalyticsAccountName` \> met de naam van een bestaande gegevens Lake Analytics-Account. De aanvraag nettolading voor deze opdracht bevindt zich in het **SubmitADLAJob.json** -bestand dat is opgegeven voor de `-d` parameter hierboven. De inhoud van het bestand input.json er dan ongeveer als volgt te werk:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "properties": {
            "type": "USql",
            "script": "@searchlog =\n    EXTRACT UserId          int,\n            Start           DateTime,\n            Region          string,\n            Query          
        string,\n            Duration        int?,\n            Urls            string,\n            ClickedUrls     string\n    FROM \"/Samples/Data/SearchLog.tsv\"\n    US
        ING Extractors.Tsv();\n\nOUTPUT @searchlog   \n    TO \"/Output/SearchLog-from-Data-Lake.csv\"\nUSING Outputters.Csv();"
        }
    }

De uitvoer is vergelijkbaar met:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "myadl@SPI",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "2016-10-05T13:54:59.9871859+00:00",
        "state": "Compiling",
        "result": "Succeeded",
        "stateAuditRecords": [
            {
            "newState": "New",
            "timeStamp": "2016-10-05T13:54:59.9871859+00:00",
            "details": "userName:myadl@SPI;submitMachine:N/A"
            }
        ],
        "properties": {
            "owner": "myadl@SPI",
            "resources": [],
            "runtimeVersion": "default",
            "rootProcessNodeId": "00000000-0000-0000-0000-000000000000",
            "algebraFilePath": "adl://myadls0831.azuredatalakestore.net/system/jobservice/jobs/Usql/2016/10/05/13/54/8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a/algebra.xml",
            "compileMode": "Semantic",
            "errorSource": "Unknown",
            "totalCompilationTime": "PT0S",
            "totalPausedTime": "PT0S",
            "totalQueuedTime": "PT0S",
            "totalRunningTime": "PT0S",
            "type": "USql"
        }
    }


## <a name="list-u-sql-jobs"></a>I-SQL-taken weergeven

De volgende opdracht uit krul ziet u hoe U SQL-taken:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

Vervang \< `REDACTED` \> met het Autorisatietoken en \< `DataLakeAnalyticsAccountName` \> met de naam van een bestaande gegevens Lake Analytics-Account. 


De uitvoer is vergelijkbaar met:

    {
    "value": [
        {
        "jobId": "65cf1691-9dbe-43cd-90ed-1cafbfb406fb",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someone@microsoft.com",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:46:53 GMT",
        "startTime": "Wed, 05 Oct 2016 13:47:33 GMT",
        "endTime": "Wed, 05 Oct 2016 13:48:07 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        },
        {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someoneadl@SPI",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:54:59 GMT",
        "startTime": "Wed, 05 Oct 2016 13:55:43 GMT",
        "endTime": "Wed, 05 Oct 2016 13:56:11 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        }
    ],
    "nextLink": null,
    "count": null
    }


## <a name="get-catalog-items"></a>Catalogusitems ophalen

De volgende opdracht uit krul ziet u hoe de databases ophalen van de catalogus:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

De uitvoer is vergelijkbaar met:

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a>Zie ook

- Een complexe query's, raadpleegt u [analyseren Website Logboeken door middel van Azure gegevens Lake analyses](data-lake-analytics-analyze-weblogs.md).
- Zie [ontwikkelen I--SQL-scripts met gegevens Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)om te beginnen ontwikkelen van toepassingen I-SQL.
- Zie [aan de slag met Azure gegevens Lake Analytics U SQL - taal](data-lake-analytics-u-sql-get-started.md)voor meer I-SQL.
- Zie [Azure gegevens Lake Analytics beheren met behulp van Azure portal](data-lake-analytics-manage-use-portal.md)voor beheertaken.
- Als u een overzicht van gegevens Lake analyses, Zie [overzicht van de Azure gegevens Lake Analytics](data-lake-analytics-overview.md).
- Klik op het tabblad-selectors boven aan de pagina overzicht van de dezelfde zelfstudie met een ander hulpprogramma.
- U wilt vastleggen van diagnostische informatie, raadpleegt u [toegang tot diagnostische logboeken van Azure gegevens Lake analysegegevens](data-lake-analytics-diagnostic-logs.md)
