<properties
    pageTitle="REST back-up en herstellen van App Service-apps gebruiken"
    description="Informatie over het gebruik van RESTful API oproepen naar een back-up en herstellen van een app in Azure App-Service"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-rest-to-back-up-and-restore-app-service-apps"></a>REST back-up en herstellen van App Service-apps gebruiken

> [AZURE.SELECTOR]
- [PowerShell](../app-service/app-service-powershell-backup.md)
- [REST API](websites-csm-backup.md)

[App Service apps](https://azure.microsoft.com/services/app-service/web/) kan worden back-up BLOB's in Azure opslag. De back-up kan ook van de app-databases bevatten. Als de app ooit per ongeluk wordt verwijderd, of als de app moet worden hersteld naar een eerdere versie, kan deze worden teruggezet uit een vorige back-up. Back-ups kunnen worden uitgevoerd op elk gewenst moment op aanvraag of back-ups met passende tussenpozen kunnen worden gepland.

In dit artikel wordt uitgelegd hoe u een back-up en herstellen van een app met RESTful API-aanvragen. Als u wilt maken en beheren van app-back-ups grafisch via de portal van Azure, raadpleegt u [Back-up van een WebApp in Azure App-Service](web-sites-backup.md)

<a name="gettingstarted"></a>
## <a name="getting-started"></a>Aan de slag
Als u wilt verzenden REST-aanvragen, moet u weten van uw app- **naam**, **resourcegroep**en **abonnements-id**. Deze informatie kunt vinden door te klikken op de app in het blad **App Service** van de [Azure-portal](https://portal.azure.com). Voor de voorbeelden in dit artikel configureert we de website **backuprestoreapiexamples.azurewebsites.net**. Er is opgeslagen in de standaard-Web-WestUS resourcegroep en klik op een abonnement met de ID 00001111-2222-3333-4444-555566667777 wordt uitgevoerd.

![Informatie over de Website van de steekproef][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>
## <a name="backup-and-restore-rest-api"></a>Back-up en herstellen REST API
Aan bod komen nu enkele voorbeelden van het gebruik van de REST API back-up maken en terugzetten van een app. Elke voorbeeld bevat de hoofdtekst van een URL en HTTP-verzoek. De URL van de steekproef bestaat uit tijdelijke aanduidingen waaromheen accolades, zoals {abonnements-id}. De tijdelijke aanduidingen vervangen door de bijbehorende gegevens voor de app. Hier volgt een beschrijving van elke tijdelijke aanduiding die wordt weergegeven in het voorbeeld-URL's voor een verwijzing naar.

* abonnement-id-ID van de Azure-abonnement met de app
* resource-groep-naam: de naam van de resourcegroep met de app
* naam: de naam van de app
* back-up-id-ID van de app back-up

Zie voor de volledige documentatie van de API, met inbegrip van verschillende optionele parameters die kunnen worden opgenomen in de HTTP-aanvraag, de [Azure Resource Explorer](https://resources.azure.com/).

<a name="backup-on-demand"></a>
## <a name="backup-an-app-on-demand"></a>Back-up maken van een app op aanvraag
Verzenden als u wilt back-up van een app direct, een **POST** -aanvraag naar **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backup/**.

Hier ziet u de URL ziet er als de website van onze voorbeeld gebruikt. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/Backup/**

Geef een JSON-object in de hoofdtekst van uw aanvraag kunt invullen om op te geven welke opslag-account om toegang te gebruiken voor de opslag van de back-up. De JSON-object moet een eigenschap met de naam **storageAccountUrl**, waarin een [SA's URL](../storage/storage-dotnet-shared-access-signature-part-1.md) schrijftoegang voor de opslag van Azure container met de back-blob toekennen. Als u back-up van uw databases wilt, moet u ook een lijst met de namen, typen en verbindingstekenreeksen met de van de databases worden back-up moet opgeven.

```
{
    "properties":
    {
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        “databases”: [
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”,
                "connectionString": "<your database connection string>"
            }
        ]
    }
}
```

Een back-up van de app wordt meteen gestart wanneer de aanvraag is ontvangen. Het back-proces duurt erg lang om te voltooien. De HTTP-antwoord bevat een ID die u kunt gebruiken in een andere aanvraag om de status van de back-up weer te geven. Hier volgt een voorbeeld van de hoofdtekst van het HTTP-antwoord op onze back-verzoek.

```
{
    "id": "/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples",
    "name": " backuprestoreapiexamples ",
    "type": "Microsoft.Web/sites",
    "location": "WestUS",
    "properties":    {
        "id": 1,
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        "blobName": “backup_201509291825.zip”,
        "name": "backup_201509291825",
        "status": 4,
        "sizeInBytes": 0,
        "created": "2015-09-29T18:25:26.3992707Z",
        "log": null,
        "databases": [
            {
                "databaseType": "SqlAzure",
                "name": "MyDatabase1",
                "connectionString": "<your database connection string>"
            }
        ],
        "scheduled": false,
        "correlationId": "ea730f4d-dd06-4f7f-a090-9aa09k662h36",
    }
}
```

>[AZURE.NOTE] Foutberichten vindt u in de eigenschap voor het registreren van het HTTP-antwoord.

<a name="schedule-automatic-backups"></a>
## <a name="schedule-automatic-backups"></a>Automatische back-ups plannen
Naast het back-ups van een app op aanvraag, kunt u ook een back-up veroorzaken, automatisch plannen.

### <a name="set-up-a-new-automatic-backup-schedule"></a>Een nieuwe planning voor automatische back-ups instellen
Verzenden als u een back-planning wilt, een aanvraag **plaatsen** om **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup**.

Hier ziet u hoe de URL voor de website van onze voorbeeld eruitziet. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/Backup**

Het hoofdgedeelte van de aanvraag moet een JSON-object dat de back-configuratie. Hier volgt een voorbeeld waarbij alle vereiste parameters.

```
{
    "location": "WestUS",
    "properties": // Represents an app restore request
    {
        "backupSchedule": { // Required for automatically scheduled backups
            "frequencyInterval": "7",
            "frequencyUnit": "Day",
            "keepAtLeastOneBackup": "True",
            "retentionPeriodInDays": "10",
        },
        "enabled": "True", // Must be set to true to enable automatic backups
        "name": “mysitebackup”,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

In dit voorbeeld wordt de app worden automatisch back-up moet elke zeven dagen geconfigureerd. De parameters **frequencyInterval** en **frequencyUnit** samen Hiermee bepaalt u hoe vaak de back-ups optreden. Geldige waarden voor **frequencyUnit** zijn **uur** en **dag**. Stel bijvoorbeeld frequencyInterval om het back-up van een app om 12 uur, 12 en frequencyUnit op uur.

Oude back-ups worden automatisch verwijderd uit het opslag-account. U kunt bepalen hoe oude de back-ups kunnen door de parameter **retentionPeriodInDays** . Als u hebben altijd ten minste één back-up opgeslagen wilt, ongeacht hoe oude is, deze **keepAtLeastOneBackup** ingesteld op waar.

### <a name="get-the-automatic-backup-schedule"></a>De planning voor automatische back-ups krijgen
Als u de back-up van een app, kunt u een **POST** -aanvraag verzendt naar de URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup/list**.

De URL voor de site van onze voorbeeld is **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.

<a name="get-backup-status"></a>
## <a name="get-the-status-of-a-backup"></a>De status van een back-up ophalen
Afhankelijk van hoe groot de app wordt uitgevoerd, kan een back-up een tijdje duren. Back-ups mogelijk ook mislukt, time-out, of gedeeltelijk mislukt. Als u wilt zien van de status van back-ups van een app, verzendt u een **GET** -aanvraag naar de URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups**.

Als u wilt zien van de status van een specifieke back-up, stuurt u een GET-aanvraag naar de URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.

Hier ziet u hoe de URL voor de website van onze voorbeeld eruitziet. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1**

De hoofdtekst van het antwoord bevat een vergelijkbaar is met dit voorbeeld JSON-object.

```
{
    "properties":  {
        "id": 1,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl",
        "blobName": "backup_201509291734.zip",
        "name": "backup_201509291734",
        "status": 2,
        "sizeInBytes": 151973,
        "created": "2015-09-29T17:34:57.2091605",
        "scheduled": false,
        "lastRestoreTimeStamp": null,
        "finishedTimeStamp": "2015-09-29T17:35:11.3293602",
        "correlationId": "2379fc13-418a-4b9c-920d-d06e73c5028d",
        "websiteSizeInBytes": 209920
    }
}
```

De status van een back-up is numeriek. Hier ziet u elke mogelijke status.

* 0 – InProgress: de back-up is gestart, maar nog niet is voltooid.
* 1 – is mislukt: De back-up is mislukt.
* 2 – is geslaagd: De back-up is voltooid.
* 3 – time-out: De back-up is niet volledig in tijd en is geannuleerd.
* 4 – gemaakt: De back-aanvraag is in de wachtrij maar niet is gestart.
* 5 – overgeslagen: De back-up is te vanwege een planning te veel back-ups activeert.
* 6 – PartiallySucceeded: De back-up is voltooid, maar sommige bestanden zijn niet back-up gemaakt omdat ze niet kunnen worden gelezen. Dit gebeurt meestal omdat een exclusief is vergrendeld de bestanden.
* 7 – DeleteInProgress: De back-up is aangevraagd zijn verwijderd, maar nog niet zijn verwijderd.
* 8 – DeleteFailed: De back-up kan niet worden verwijderd. Dit kan gebeuren omdat de SA's-URL die is gebruikt voor het maken van de back-up is verlopen.
* 9 – verwijderd: De back-up is verwijderd.

<a name="restore-app"></a>
## <a name="restore-an-app-from-a-backup"></a>Een app herstellen uit een back-up
Als uw app is verwijderd, of als u wilt herstellen van uw app naar een eerdere versie, kunt u de app terugzetten uit een back-up. Om aan te roepen een herstellen, kunt u een **POST** -aanvraag verzendt naar de URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/restore**.

Hier ziet u hoe de URL voor de website van onze voorbeeld eruitziet. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1/Restore**

Verzend een JSON-object dat de eigenschappen voor de bewerking voor terugzetten in de hoofdtekst van het verzoek. Hier volgt een voorbeeld met alle vereiste eigenschappen:

```
{
    "location": "WestUS",
    "properties":
    {
        "blobName": "backup_201509280431.zip",
        "databases": [ // Not required unless you’re restoring databases
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”
        }],
        "overwrite": "true",
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl" // SAS URL to storage container containing your website backup
    }
}
```

### <a name="restore-to-a-new-app"></a>U wilt een nieuwe app herstellen
Soms wilt u mogelijk een nieuwe app maken wanneer u een back-up, in plaats van een bestaand app overschrijven terugzet. Klik hiertoe wijzigen van de aanvraag-URL te laten verwijzen naar de nieuwe app die u wilt maken en wijzigen van de eigenschap **overschrijven** in de JSON in **Onwaar**.

<a name="delete-app-backup"></a>
## <a name="delete-an-app-backup"></a>Een back-up van app verwijderen
Als u verwijderen van een back-up wilt, stuurt u een verzoek om te **verwijderen** naar de URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.

Hier ziet u hoe de URL voor de website van onze voorbeeld eruitziet. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1**

<a name="manage-sas-url"></a>
## <a name="manage-a-backups-sas-url"></a>URL van een back-up SA's beheren
Azure App-Service probeert te verwijderen van de back-up van Azure-opslag via de URL SA's die u hebt ontvangen wanneer de back-up is gemaakt. Als deze URL SA's niet langer geldig is, kan de back-up kan niet worden verwijderd via de REST API. U kunt echter de URL van de SA's die zijn gekoppeld aan een back-up te maken door een verzoek voor een **bericht** sturen naar de URL bijwerken **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.

Hier ziet u hoe de URL voor de website van onze voorbeeld eruitziet. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1/List**

Verzend een JSON-object dat de nieuwe SA's-URL bevat in de hoofdtekst van het verzoek. Hier volgt een voorbeeld.

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

>[AZURE.NOTE] De URL van de SA's die zijn gekoppeld aan een back-up is niet veiligheidsredenen worden geretourneerd bij het verzenden van een GET-aanvraag voor een specifieke back-up. Als u weergeven van de URL van de SA's die zijn gekoppeld aan een back-up wilt, stuurt u een verzoek voor een bericht naar de bovenstaande URL. Een leeg JSON-object in het hoofdgedeelte van de aanvraag opnemen. De reactie van de server bevat alle dat back-up-informatie, inclusief de URL SA's.

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
