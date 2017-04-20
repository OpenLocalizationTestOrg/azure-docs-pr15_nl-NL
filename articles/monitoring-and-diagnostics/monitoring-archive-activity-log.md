<properties
    pageTitle="Het activiteitenlogboek Azure archiveert | Microsoft Azure"
    description="Leer hoe u uw Azure-logboek voor langdurige bewaarbeleid in een account opslag archiveren."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="johnkem"/>

# <a name="archive-the-azure-activity-log"></a>Het activiteitenlogboek Azure archiveren
In dit artikel zien we hoe u de Azure-portal PowerShell-Cmdlets of meerdere platforms CLI archiveren van uw [**Azure activiteit Log**](monitoring-overview-activity-logs.md) in een account opslag kunt gebruiken. Deze optie is handig als u wilt bewaren van uw activiteit Log langer dan 90 dagen (met volledige controle over het bewaarbeleid) voor controle, statische analyse of back-up. Als u alleen moet uw gebeurtenissen 90 dagen worden bewaard of minder u niet hoeft voor het instellen van archivering aan een account opslag, aangezien activiteit gebeurtenissen in het Azure platform 90 dagen behouden blijven zonder in te schakelen archiveren.

## <a name="prerequisites"></a>Vereisten voor
Voordat u begint, moet u [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account) waarnaar u uw activiteit Log kunt archiveren. Het is raadzaam dat u een bestaande opslag-account met andere, niet-bewaken gegevens opgeslagen in deze zodat u toegang tot gegevens bewaken beter kunt bepalen niet gebruiken. Als u diagnostische logboeken en maatstelsel een opslag-account ook archiveert, deze mogelijk nog wel handig om de dat account opslagruimte voor uw logboek ook gebruiken om alle controlegegevens in een centrale locatie. De opslagruimte waarmee u moet een algemene opslag-account, niet een blob storage-account.

## <a name="log-profile"></a>Log profiel
Als u wilt archiveren het logboek met een van de onderstaande methoden, moet u het **Logboek profiel** voor een abonnement instellen. Het profiel Log definieert het type gebeurtenissen die zijn opgeslagen of streamen en de uitvoer, opslag-account en/of gebeurtenis hub. Het bewaarbeleid (het aantal dagen worden bewaard) wordt ook gedefinieerd voor gebeurtenissen die zijn opgeslagen in een opslag-account. Als het bewaarbeleid is ingesteld op nul, wordt gebeurtenissen worden voor onbepaalde tijd opgeslagen. Dit kan anders worden ingesteld op een waarde tussen 1 en 2147483647. [Kunt u meer informatie over log profielen hier](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles).

## <a name="archive-the-activity-log-using-the-portal"></a>Het logboek met behulp van de portal archiveren
1. Klik op de koppeling **Gebeurtenislogboek** op de navigatiebalk van de tabel aan de linkerkant in de portal. Als u een koppeling voor het logboek niet ziet, klikt u eerst op de koppeling **Meer Services** .

    ![Ga naar het gebeurtenissenlogboek blade](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. Aan de bovenkant van het blad, klikt u op **exporteren**.

    ![Klik op de knop exporteren](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. In het blad dat wordt weergegeven, schakel het selectievakje voor het **exporteren naar een opslag-account** en selecteer een opslag-account.

    ![Een opslag-account instellen](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. Met de schuifregelaar of het tekstvak, een aantal dagen waarvoor activiteit gebeurtenissen moeten worden bewaard in uw account opslag definiëren. Als u liever uw gegevens op de voor onbepaalde tijd doorgevoerd in het account opslag, stelt u dit nummer op nul.
5. Klik op **Opslaan**.

## <a name="archive-the-activity-log-via-powershell"></a>Het gebeurtenissenlogboek via PowerShell archiveren
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus -RetentionInDays 180 -Categories Write,Delete,Action
```

| Eigenschap         | Vereist | Beschrijving                                                                                                                                                                                                                                                                                       |
|------------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| StorageAccountId | Nee       | Resource-ID van het opslag-Account waaraan activiteitenlogboeken moeten worden opgeslagen.                                                                                                                                                                                                                        |
| Locaties        | Ja      | Door komma's gescheiden lijst met regio's waarvan u wilt verzamelen activiteit gebeurtenissen. U kunt een lijst met alle regio's [door naar deze pagina te gaan](https://azure.microsoft.com/en-us/regions) of met behulp van [De Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx)weergeven. |
| RetentionInDays  | Ja      | Het aantal dagen voor welke gebeurtenissen moeten worden bewaard, tussen 1 en 2147483647. De waarde nul worden opgeslagen de logboeken voor onbepaalde tijd (eeuwen).                                                                                                                                                                                             |
| Categorieën       | Ja      | Door komma's gescheiden lijst met gebeurteniscategorieën die moet worden verzameld. Mogelijke waarden zijn schrijven, verwijderen en actie.                                                                                                                                                                                 |
## <a name="archive-the-activity-log-via-cli"></a>Het gebeurtenissenlogboek via CLI archiveren
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --locations global,westus,eastus,northeurope --retentionInDays 180 –categories Write,Delete,Action
```

| Eigenschap        | Vereist | Beschrijving                                                                                                                                                                                                                                                                                       |
|-----------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| naam            | Ja      | De naam van uw profiel log.                                                                                                                                                                                                                                                                         |
| storageId       | Nee       | Resource-ID van het opslag-Account waaraan activiteitenlogboeken moeten worden opgeslagen.                                                                                                                                                                                                                        |
| locaties       | Ja      | Door komma's gescheiden lijst met regio's waarvan u wilt verzamelen activiteit gebeurtenissen. U kunt een lijst met alle regio's [door naar deze pagina te gaan](https://azure.microsoft.com/en-us/regions) of met behulp van [De Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx)weergeven. |
| retentionInDays | Ja      | Het aantal dagen voor welke gebeurtenissen moeten worden bewaard, tussen 1 en 2147483647. De waarde nul wordt de logboeken voor onbepaalde tijd opgeslagen (eeuwen).                                                                                                                                                                                             |
| categorieën      | Ja      | Door komma's gescheiden lijst met gebeurteniscategorieën die moet worden verzameld. Mogelijke waarden zijn schrijven, verwijderen en actie.                                                                                                                                                                                 |

## <a name="storage-schema-of-the-activity-log"></a>Schema van de opslag van het logboek
Zodra u hebt ingesteld archivering, wordt een container opslag gemaakt in het account opslag zodra een logboek gebeurtenis plaatsvindt. De BLOB's in de container Volg dezelfde opmaak in het logboek met activiteit en de diagnostische logboeken. De structuur van deze BLOB's luidt als volgt:

> inzichten-operationele-logboeken/name = standaard/resourceId = / ABONNEMENTEN / {abonnements-ID} / y = {viercijferige numerieke jaar} / m = {tweecijferige numerieke maand} / d = {tweecijferige numerieke dag} / h = {twee cijfers 24-uurs klok hour}/m=00/PT1H.json

Bijvoorbeeld de naam van een blob mogelijk:

> Insights-Operational-Logs/name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.JSON

Elke blob PT1H.json bevat een blob JSON van gebeurtenissen die hebben plaatsgevonden binnen het uur opgegeven in de blob-URL (bijvoorbeeld h = 12). Gebeurtenissen worden toegevoegd aan het bestand PT1H.json tijdens het presenteren uur, terwijl ze voorkomen. De minuut waarde (m = 00) is altijd 00, aangezien activiteit gebeurtenissen worden onderverdeeld in afzonderlijke BLOB's per uur.

Elke gebeurtenis wordt in het bestand PT1H.json opgeslagen in de matrix 'records', deze indeling te volgen:

```
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
                "claims": {
                    "aud": "https://management.core.windows.net/",
                    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
                    "iat": "1421876371",
                    "nbf": "1421876371",
                    "exp": "1421880271",
                    "ver": "1.0",
                    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
                    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
                    "puid": "20030000801A118C",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
                    "name": "John Smith",
                    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
                    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
                    "appidacr": "2",
                    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
                    "http://schemas.microsoft.com/claims/authnclassreference": "1"
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
            }
        }
    ]
}
```


| De elementnaam van het    | Beschrijving                                                                                                    |
|-----------------|----------------------------------------------------------------------------------------------------------------|
| tijd            | Tijdstip waarop de gebeurtenis is gegenereerd door het verwerken van de aanvraag overeenkomt van de gebeurtenis Azure-service.    |
| resourceId      | Resource-ID van de risico resource.                                                                          |
| operationName   | De naam van de bewerking.                                                                                         |
| categorie        | Categorie van de actie, bijvoorbeeld. Schrijven, lezen, actie.                                                               |
| resultType      | Het type van het resultaat, bijvoorbeeld. SUCCESS, mislukt, Start                                                            |
| resultSignature | Is afhankelijk van het resourcetype.                                                                                  |
| durationMs      | Duur van de bewerking (in milliseconden)                                                                      |
| callerIpAddress | IP-adres van de gebruiker die de bewerking, UPN-claim of name claim op basis van beschikbaarheid heeft uitgevoerd.         |
| correlationId   | Meestal een GUID in de indeling van de tekenreeks. Gebeurtenissen die delen van een correlationId deel uitmaakt van dezelfde uber actie.         |
| identiteit        | JSON blob met een beschrijving van de autorisatie en claims.                                                             |
| autorisatie   | BLOB van RBAC eigenschappen van de gebeurtenis. Meestal bevat de eigenschappen "actie", "rol" en "bereik".            |
| niveau           | Niveau van de gebeurtenis. Een van de volgende waarden: 'kritieke","Fout","Waarschuwing", 'Ter informatie' en"Uitgebreid" |
| locatie        | Regio in de locatie is opgetreden (of globale).                                                             |
| Eigenschappen      | Set `<Key, Value>` paren (dat wil zeggen woordenlijst) met een beschrijving van de details van de gebeurtenis.                             |

> [AZURE.NOTE] De eigenschappen en het gebruik van deze eigenschappen kunnen variëren afhankelijk van de resource.

## <a name="next-steps"></a>Volgende stappen
- [BLOB's voor analyse downloaden](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)
- [Het logboek met gebeurtenis Hubs streamen](monitoring-stream-activity-logs-event-hubs.md)
- [Lees meer over het logboek](monitoring-overview-activity-logs.md)
