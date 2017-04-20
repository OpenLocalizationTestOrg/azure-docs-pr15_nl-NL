<properties 
   pageTitle="Aan de slag met Lake gegevensopslag met REST API | Microsoft Azure" 
   description="WebHDFS REST API's gebruiken om u te bewerkingen uitvoeren op Lake gegevensopslag" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a>Aan de slag met Azure Lake gegevensopslag REST API's gebruiken

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

In dit artikel leert u hoe u WebHDFS REST API's en gegevens Lake Store REST API's gebruikt om uit te voeren accountbeheer, evenals bestandssysteem bewerkingen op Azure Lake gegevensopslag. Azure Lake gegevensopslag beschrijft een eigen REST API's van het account beheer. Omdat Lake gegevensopslag compatibel met HDFS en Hadoop-systeem is, ondersteunt dit echter WebHDFS REST API's gebruiken in bestandssysteem bewerkingen.

>[AZURE.NOTE] Ondersteuning voor gegevensopslag Lake voor gedetailleerde informatie over de REST API: Zie [Azure gegevens Lake Store REST API tablets](https://msdn.microsoft.com/library/mt693424.aspx).

## <a name="prerequisites"></a>Vereisten voor

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

- **Een Azure Active Directory-toepassing maken**. De Azure AD-toepassing kunt u de toepassing Lake gegevensopslag met Azure AD verifiëren. Er zijn verschillende manieren om te verifiëren met Azure AD, welke **eindgebruikers** of **service-naar-service-verificatie**. Zie voor instructies en meer informatie over het om te verifiëren, [verifiëren met Lake gegevensopslag Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

- [omslaan](http://curl.haxx.se/). In dit artikel wordt krul gebruikt om te laten zien hoe u REST API oproepen tegen een Lake gegevensopslag-account.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Hoe ik verifiëren met behulp van Azure Active Directory?

U kunt op twee manieren gebruiken om te verifiëren met Azure Active Directory.

### <a name="end-user-authentication-interactive"></a>Eindgebruikers verificatie (interactieve)

In dit scenario wordt gevraagd of de gebruiker aan te melden en alle bewerkingen in de context van de gebruiker worden uitgevoerd. De volgende stappen uitvoeren voor interactieve verificatie.

1. Via de toepassing, door de gebruiker te omleiden naar de volgende URL:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<REDIRECT-URI > moet worden gecodeerd voor gebruik in een URL. Ja, voor https://localhost, gebruikt u `https%3A%2F%2Flocalhost`)

    Voor de toepassing van deze zelfstudie kunt u de tijdelijke aanduiding voor waarden in de URL hierboven vervangen en plak deze in de adresbalk van de webbrowser. U wordt omgeleid als u wilt verifiëren met behulp van uw Azure login. Nadat u zich met succes hebt aangemeld, wordt het antwoord wordt weergegeven in de adresbalk van de browser. Het antwoord niet in de volgende indeling:
        
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

In dit scenario de de toepassing biedt een eigen referenties om de bewerkingen uitvoeren. Hiervoor moet u een POST-aanvraag zoals wordt weergegeven onder verlenen. 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

De uitvoer van deze aanvraag bevat een Autorisatietoken (aangeduid met `access-token` in de onderstaande uitvoer) die u vervolgens met uw oproepen REST API wordt doorgegeven. Deze verificatietoken opslaan in een tekstbestand; Dit moet u verderop in dit artikel.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

In dit artikel worden gebruikt de **niet-interactieve** methode. Zie voor meer informatie over niet-interactieve (service-naar-service oproepen), [Service aan service gesprekken met referenties](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="create-a-data-lake-store-account"></a>Een account voor gegevensopslag Lake maken

Deze bewerking is gebaseerd op de REST API gesprek gedefinieerd [hier](https://msdn.microsoft.com/library/mt694078.aspx).

Gebruik de volgende opdracht uit krul. Vervang ** \<yourstorename >** met de naam van uw Lake gegevensopslag.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

Vervang in het bovenstaande opdracht \< `REDACTED` \> met het Autorisatietoken opgehaald u eerder. De aanvraag nettolading voor deze opdracht bevindt zich in het **input.json** -bestand dat is opgegeven voor de `-d` parameter hierboven. De inhoud van het bestand input.json er dan ongeveer als volgt te werk:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }   

## <a name="create-folders-in-a-data-lake-store-account"></a>Mappen maken in een account voor gegevensopslag Lake

Deze bewerking is gebaseerd op de WebHDFS REST API gesprek gedefinieerd [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).

Gebruik de volgende opdracht uit krul. Vervang ** \<yourstorename >** met de naam van uw Lake gegevensopslag.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS

Vervang in het bovenstaande opdracht \< `REDACTED` \> met het Autorisatietoken opgehaald u eerder. Deze opdracht maakt een map **mytempdir** onder de hoofdmap van uw account voor gegevensopslag Lake.

U ziet een antwoord als volgt als de bewerking voltooid is:

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a>Lijst met mappen in een account voor gegevensopslag Lake

Deze bewerking is gebaseerd op de WebHDFS REST API gesprek gedefinieerd [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).

Gebruik de volgende opdracht uit krul. Vervang ** \<yourstorename >** met de naam van uw Lake gegevensopslag.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS

Vervang in het bovenstaande opdracht \< `REDACTED` \> met het Autorisatietoken opgehaald u eerder.

U ziet een antwoord als volgt als de bewerking voltooid is:

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "NotSupportYet",
            "group": "NotSupportYet"
        }]
    }
    }

## <a name="upload-data-into-a-data-lake-store-account"></a>Gegevens importeren in een account voor gegevensopslag Lake uploaden

Deze bewerking is gebaseerd op de WebHDFS REST API gesprek gedefinieerd [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).

Gegevens met behulp van de WebHDFS REST API uploaden, is dat een proces, zoals hieronder wordt uitgelegd.

1. Een verzoek om te ZETTEN HTTP verzenden zonder het verzenden van de bestandsgegevens te uploaden. Vervang in de volgende opdracht ** \<yourstorename >** met de naam van uw Lake gegevensopslag.

        curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=CREATE

    De uitvoer van deze opdracht worden bevat de URL van een tijdelijke omleiding, zoals een hieronder wordt weergegeven.

        HTTP/1.1 100 Continue

        HTTP/1.1 307 Temporary Redirect
        ...
        ...
        Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=CREATE&write=true
        ...
        ...

2. U moet nu een andere HTTP plaatsen aanvraag ten opzichte van de URL die wordt weergegeven voor de eigenschap **locatie** in het antwoord indienen. Vervang ** \<yourstorename >** met de naam van uw Lake gegevensopslag.

        curl -i -X PUT -T myinputfile.txt -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=CREATE&write=true

    De uitvoer is ongeveer als volgt uit:

        HTTP/1.1 100 Continue

        HTTP/1.1 201 Created
        ...
        ...

## <a name="read-data-from-a-data-lake-store-account"></a>Gegevens van een account voor gegevensopslag Lake lezen

Deze bewerking is gebaseerd op de WebHDFS REST API gesprek gedefinieerd [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).

Het lezen van gegevens uit een gegevensopslag Lake is account een proces.

* U eerst een GET-aanvraag ten opzichte van het eindpunt verzenden `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`. Hiermee herstelt u een locatie voor het indienen van de volgende GET-aanvraag aan.
* Foutenrapporten vervolgens de GET-aanvraag ten opzichte van het eindpunt `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`. Hiermee wordt de inhoud van het bestand weergegeven.

Echter, omdat er geen verschil tussen de invoerparameters tussen de eerste en de tweede stap, kunt u de `-L` -parameter voor de eerste aanvraag indienen. `-L`optie is in principe combineert twee aanvragen op een en ervoor zorgt dat krul opnieuw uitvoeren van de aanvraag op de nieuwe locatie. Ten slotte de uitvoer van alle verzoek gesprekken wordt weergegeven, zoals hieronder weergegeven. Vervang ** \<yourstorename >** met de naam van uw Lake gegevensopslag.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN

Hier ziet u een uitvoer ongeveer als volgt uit:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...
    
    HTTP/1.1 200 OK
    ...
    
    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a>De naam van een bestand in een account Lake gegevensopslag wijzigen

Deze bewerking is gebaseerd op de WebHDFS REST API gesprek gedefinieerd [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).

De volgende krul-opdracht om de naam van een bestand te gebruiken. Vervang ** \<yourstorename >** met de naam van uw Lake gegevensopslag.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt

Hier ziet u een uitvoer ongeveer als volgt uit:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a>Een bestand verwijderen uit een account voor gegevensopslag Lake

Deze bewerking is gebaseerd op de WebHDFS REST API gesprek gedefinieerd [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).

Gebruik de volgende opdracht uit krul een bestand te verwijderen. Vervang ** \<yourstorename >** met de naam van uw Lake gegevensopslag.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE

Hier ziet u een uitvoer als volgt uit:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a>Een account voor gegevensopslag Lake verwijderen

Deze bewerking is gebaseerd op de REST API gesprek gedefinieerd [hier](https://msdn.microsoft.com/library/mt694075.aspx).

Gebruik de volgende opdracht uit krul een account voor gegevensopslag Lake verwijderen. Vervang ** \<yourstorename >** met de naam van uw Lake gegevensopslag.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Hier ziet u een uitvoer als volgt uit:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a>Zie ook

- [Bron Big Data-toepassingen die compatibel is met Azure Lake gegevensopslag openen](data-lake-store-compatible-oss-other-applications.md)
 
