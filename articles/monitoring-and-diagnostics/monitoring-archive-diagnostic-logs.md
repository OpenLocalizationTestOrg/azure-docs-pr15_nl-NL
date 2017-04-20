<properties
    pageTitle="Azure diagnostische logboeken archiveren | Microsoft Azure"
    description="Leer hoe u uw Azure diagnostische logboeken voor lange bewaarbeleid in een account opslag archiveren."
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
    ms.date="08/26/2016"
    ms.author="johnkem"/>

# <a name="archive-azure-diagnostic-logs"></a>Azure diagnostische logboeken archiveren
In dit artikel ziet hoe u de Azure-portal PowerShell-Cmdlets, CLI of REST API kunt gebruiken om te archiveren van uw [Azure diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md) in een opslag-account. Deze optie is handig als u wilt bewaren van uw diagnostische logboeken met een optionele bewaarbeleid voor controle, statische analyse of back-up.

## <a name="prerequisites"></a>Vereisten voor
Voordat u begint, moet u [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account) waarnaar u uw diagnostische logboeken kunt archiveren. Het is raadzaam dat u een bestaande opslag-account met andere, niet-bewaken gegevens opgeslagen in deze zodat u toegang tot gegevens bewaken beter kunt bepalen niet gebruiken. Als u ook van uw activiteit Log en diagnostische maatstelsel een opslag-account archiveren, deze mogelijk nog wel handig om de dat account opslagruimte voor uw diagnostische logboeken ook gebruiken om alle controlegegevens in een centrale locatie. De opslagruimte waarmee u moet een algemene opslag-account, niet een blob storage-account.

## <a name="diagnostic-settings"></a>Diagnostische instellingen
Als u wilt archiveren uw diagnostische logboeken met een van de onderstaande methoden, moet u een **Diagnostische instelling** voor een bepaalde resource instellen. Een diagnose stellen voor een resource de categorieën van definieert logboeken die zijn die zijn opgeslagen of streamen en de uitvoer, opslag-account en/of gebeurtenis hub. Het bewaarbeleid (het aantal dagen worden bewaard) wordt ook gedefinieerd voor gebeurtenissen van elke categorie log is opgeslagen in een opslag-account. Als een bewaarbeleid is ingesteld op nul, worden gebeurtenissen voor die categorie log opgeslagen voor onbepaalde tijd (dat wil zeggen, eeuwen). Een bewaarbeleid is anders een willekeurig aantal dagen tussen 1 en 2147483647. [Kunt u meer informatie over Diagnostische instellingen hier](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).

## <a name="archive-diagnostic-logs-using-the-portal"></a>De diagnostische logboeken archiveren met behulp van de portal

1. Klik in de portal op in het blad resource voor de resource is waarop u wilt inschakelen archiveren van diagnostische logboeken.
2. Selecteer in de sectie **controle** van het instellingenmenu de resource en **Diagnostische gegevens**.

    ![Sectie van het menu van de resource bewaken](media/monitoring-archive-diagnostic-logs/diag-log-monitoring-sec.png)
3. Schakel het selectievakje voor het **exporteren naar opslag-Account**en selecteer vervolgens een opslag-account. Stel desgewenst een aantal dagen worden bewaard deze logboeken met behulp van de **bewaarbeleid (dagen)** schuifregelaars. Een bewaarbeleid van nul dagen slaat de logboeken voor onbepaalde tijd.

    ![Diagnostische logboeken blade](media/monitoring-archive-diagnostic-logs/diag-log-monitoring-blade.png)
4. Klik op **Opslaan**.

Diagnostische logboeken worden gearchiveerd aan dat account opslag zodra nieuwe gebeurtenisgegevens wordt gegenereerd.

## <a name="archive-diagnostic-logs-via-the-powershell-cmdlets"></a>Diagnostische logboeken archief via de PowerShell-Cmdlets

```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| Eigenschap         | Vereist | Beschrijving                                                                                           |
|------------------|----------|-------------------------------------------------------------------------------------------------------|
| ResourceId       | Ja      | Resource-ID van de resource is waarop u wilt een diagnostische instellen.                            |
| StorageAccountId | Nee       | Resource-ID van het opslag-Account waaraan de diagnostische logboeken moeten worden opgeslagen.                          |
| Categorieën       | Nee       | Door komma's gescheiden lijst met categorieën log om in te schakelen.                                                     |
| Ingeschakeld          | Ja      | Booleaanse waarde die aangeeft of diagnostische hulpprogramma's zijn ingeschakeld of uitgeschakeld op deze resource.                  |
| RetentionEnabled | Nee       | Booleaanse waarde die aangeeft dat u als een bewaarbeleid zijn ingeschakeld voor deze resource.                            |
| RetentionInDays  | Nee       | Het aantal dagen waarvoor gebeurtenissen tussen 1 en 2147483647 moeten worden bewaard. De waarde nul worden de logboeken voor onbepaalde tijd opgeslagen. |

## <a name="archive-the-activity-log-via-the-cross-platform-cli"></a>Het gebeurtenissenlogboek via de CLI platforms archiveren

```
azure insights diagnostic set --resourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg --storageId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage –categories networksecuritygroupevent,networksecuritygrouprulecounter --enabled true
```

| Eigenschap         | Vereist | Beschrijving                                                                                           |
|------------------|----------|-------------------------------------------------------------------------------------------------------|
| resourceId       | Ja      | Resource-ID van de resource is waarop u wilt een diagnostische instellen.                            |
| storageId        | Nee       | Resource-ID van het opslag-Account waaraan de diagnostische logboeken moeten worden opgeslagen.                          |
| categorieën       | Nee       | Door komma's gescheiden lijst met categorieën log om in te schakelen.                                                     |
| ingeschakeld          | Ja      | Booleaanse waarde die aangeeft of diagnostische hulpprogramma's zijn ingeschakeld of uitgeschakeld op deze resource.                  |

## <a name="archive-diagnostic-logs-via-the-rest-api"></a>Diagnostische logboeken archief via de REST API
[In dit document bekijken](https://msdn.microsoft.com/library/azure/dn931931.aspx) voor meer informatie over hoe u een diagnostische instelling voor het gebruik van de Azure Monitor REST API kan instellen.

## <a name="schema-of-diagnostic-logs-in-the-storage-account"></a>Schema van diagnostische logboeken in de opslagruimte-account
Zodra u hebt ingesteld archivering, wordt een container opslag gemaakt in het account opslag zodra een gebeurtenis plaatsvindt in een van de log categorieën die u hebt ingeschakeld. De BLOB's in de container Volg dezelfde opmaak in de diagnostische logboeken en het logboek. De structuur van deze BLOB's luidt als volgt:

> inzichten - logboeken-{categorienaam log} / resourceId = / ABONNEMENTEN / {abonnements-ID} /RESOURCEGROUPS/ {Resourcegroepnaam} /PROVIDERS/ {provider resourcenaam} / {resource Typ} / {resourcenaam} / y = {viercijferige numerieke jaar} / m = {tweecijferige numerieke maand} / d = {tweecijferige numerieke dag} / h = {twee cijfers 24-uurs klok hour}/m=00/PT1H.json

Of, eenvoudigweg

> inzichten - logboeken-{categorienaam log} / resourceId = / {resource Id} / y = {numerieke jaartal van vier cijfers} / m = {tweecijferige numerieke maand} / d = {tweecijferige numerieke dag} / h = {twee cijfers 24-uurs klok hour}/m=00/PT1H.json

Bijvoorbeeld de naam van een blob mogelijk:

> insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json

Elke blob PT1H.json bevat een blob JSON van gebeurtenissen die hebben plaatsgevonden binnen het uur opgegeven in de blob-URL (bijvoorbeeld h = 12). Gebeurtenissen worden toegevoegd aan het bestand PT1H.json tijdens het presenteren uur, terwijl ze voorkomen. De minuut waarde (m = 00) is altijd 00, omdat de diagnostische logboeken gebeurtenissen worden onderverdeeld in afzonderlijke BLOB's per uur.

Elke gebeurtenis wordt in het bestand PT1H.json opgeslagen in de matrix 'records', deze indeling te volgen:

```
{
    "records": [
        {
            "time": "2016-07-01T00:00:37.2040000Z",
            "systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1",
            "category": "NetworkSecurityGroupRuleCounter",
            "resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG",
            "operationName": "NetworkSecurityGroupCounters",
            "properties": {
                "vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}",
                "subnetPrefix": "10.3.0.0/24",
                "macAddress": "000123456789",
                "ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp",
                "direction": "In",
                "type": "allow",
                "matchedConnections": 1988
            }
        }
    ]
}
```

| De elementnaam van het  | Beschrijving                                                                                                 |
|---------------|-------------------------------------------------------------------------------------------------------------|
| tijd          | Tijdstip waarop de gebeurtenis is gegenereerd door het verwerken van de aanvraag overeenkomt van de gebeurtenis Azure-service. |
| resourceId    | Resource-ID van de risico resource.                                                                       |
| operationName | De naam van de bewerking.                                                                                      |
| categorie      | Log categorie van de gebeurtenis.                                                                                  |
| Eigenschappen    | Set `<Key, Value>` paren (dat wil zeggen woordenlijst) met een beschrijving van de details van de gebeurtenis.                            |

> [AZURE.NOTE] De eigenschappen en het gebruik van deze eigenschappen kunnen variëren afhankelijk van de resource.

## <a name="next-steps"></a>Volgende stappen
- [BLOB's voor analyse downloaden](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)
- [Stream diagnostische logboeken op gebeurtenis Hubs](monitoring-stream-diagnostic-logs-to-event-hubs.md)
- [Lees meer over de diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md)
