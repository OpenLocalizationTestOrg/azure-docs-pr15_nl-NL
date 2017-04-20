<properties
    pageTitle="Azure belangrijke kluis logboekregistratie | Microsoft Azure"
    description="Met deze zelfstudie kunt u aan de slag met Azure-toets kluis vastleggen."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/31/2016"
    ms.author="cabailey"/>

# <a name="azure-key-vault-logging"></a>Azure belangrijke kluis logboekregistratie #
Azure-toets kluis is beschikbaar in de meeste regio's. Zie de [sleutel kluis prijzen pagina](https://azure.microsoft.com/pricing/details/key-vault/)voor meer informatie.

## <a name="introduction"></a>Inleiding  
Nadat u een of meer belangrijke kluizen hebt gemaakt, wordt waarschijnlijk wilt u controleren hoe en wanneer uw belangrijkste kluizen toegankelijk is, en door wie zijn. U kunt dit doen door het inschakelen van logboekregistratie voor sleutel kluis, waarop informatie wordt opgeslagen in een Azure opslag-account dat u opgeeft. Een nieuwe container met de naam **inzichten-logboeken-auditevent** wordt automatisch gemaakt voor uw account opgegeven opslag en kunt u dit dezelfde opslag-account voor het verzamelen van Logboeken voor meerdere belangrijke kluizen.

U kunt uw logboekinformatie maximaal openen, 10 minuten na de toets kluis bewerking. In de meeste gevallen wordt het sneller dan deze zijn.  Is het aan u de logboeken in uw account opslag beheren:

- Standaard Azure toegang besturingselement methoden voor het beveiligen van uw logboeken door te beperken wie toegang heeft tot deze gebruiken.
- Logboeken die u niet langer wilt behouden in uw account opslagruimte verwijderen.

Gebruik deze zelfstudie om u te helpen aan de slag met Azure-toets kluis vastleggen, om het maken van uw account opslag, logboekregistratie inschakelt en interpreteren van de logboekregistratie-gegevens die worden verzameld.  


>[AZURE.NOTE]  Deze zelfstudie is niet inbegrepen voor instructies voor het maken van belangrijke kluizen, sleutels of geheimen. Zie [aan de slag met Azure toets kluis](key-vault-get-started.md)voor deze informatie. Of voor de opdrachtregel platforms instructies, raadpleegt u [deze gelijkwaardige zelfstudie](key-vault-manage-with-cli.md).
>
>U configureren niet op dit moment Azure-toets kluis in de portal van Azure. Gebruik in plaats daarvan deze instructies Azure PowerShell.

De logboeken die u verzamelen kunnen met behulp van Log analytics uit de bewerkingen Management Suite visualiseren. Zie [Azure toets kluis (Preview)-oplossing in Log Analytics](../log-analytics/log-analytics-azure-key-vault.md)voor meer informatie.

Zie voor informatie over Azure-toets kluis [Wat Azure-toets kluis is?](key-vault-whatis.md)

## <a name="prerequisites"></a>Vereisten voor

Als u wilt deze zelfstudie hebt voltooid, hebt u het volgende:

- Een bestaande belangrijke kluis die u hebt gebruikt.  
- Azure PowerShell, **minimale versie van 1.0.1**. Azure PowerShell installeren en deze koppelen aan uw Azure-abonnement: Zie [installeren en configureren van Azure PowerShell](../powershell-install-configure.md). Als u Azure PowerShell al hebt geïnstalleerd en de versie van de Azure PowerShell-console niet weet, typt u `(Get-Module azure -ListAvailable).Version`.  
- Voldoende opslagruimte op Azure voor uw sleutel kluis Logboeken.


## <a id="connect"></a>Verbinding maken met uw abonnementen ##

Een Azure PowerShell-sessie starten en meld u aan bij uw Azure-account met de volgende opdracht uit:  

    Login-AzureRmAccount

Voer uw gebruikersnaam in te voeren Azure-account en wachtwoord in het pop-browservenster. Azure PowerShell krijgt alle abonnementen die zijn gekoppeld aan dit account en al dan niet standaard, gebruikt de eerste fase.

Als u meerdere abonnementen hebt, is het wellicht geeft u aan een specifieke een die is gebruikt voor het maken van uw Azure-toets kluis. Typ het volgende als u wilt zien van de abonnementen voor uw account:

    Get-AzureRmSubscription

Als u wilt opgeven van het abonnement dat is gekoppeld aan uw belangrijkste kluis die u meldt zich, typ:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Zie voor meer informatie over het configureren van Azure PowerShell [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).


## <a id="storage"></a>Een nieuw account in de opslagruimte voor uw logboeken maken ##

Hoewel u een bestaand account in de opslagruimte voor uw Logboeken gebruiken kunt, gaan we een nieuw account voor de opslag dat wordt worden toegewezen aan de sleutel kluis logboeken maken. Voor het gemak voor wanneer we hebben dit later aangeven slaan we de details in een variabele met de naam **sa**.

Aanvullende vereenvoudigen management, we dezelfde resourcegroep ook gebruiken als de database met onze belangrijke kluis. Deze resourcegroep heet **ContosoResourceGroup** van de [handleiding aan de slag](key-vault-get-started.md)en zullen wij de locatie van Oost-Azië gebruiken. Vervang deze waarden voor uw eigen, indien van toepassing:

    $sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup -Name ContosoKeyVaultLogs -Type Standard_LRS -Location 'East Asia'


>[AZURE.NOTE]  Als u wilt een bestaand account voor de opslag gebruiken, moet deze hetzelfde abonnement als uw belangrijkste kluis gebruiken en moet deze het implementatiemodel resourcemanager, in plaats van het model Klassiek implementatie gebruiken.

## <a id="identify"></a>De belangrijkste kluis voor uw logboeken identificeren ##

In ons ophalen gestart zelfstudie is de naam van onze belangrijke kluis **ContosoKeyVault**, zodat we blijft u die naam gebruiken en de details opslaan in een variabele met de naam **kv**:

    $kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'


## <a id="enable"></a>Logboekregistratie inschakelen ##

Als u wilt inschakelen van logboekregistratie voor sleutel kluis gebruiken we de cmdlet Set-AzureRmDiagnosticSetting, samen met de variabelen die u hebt gemaakt voor onze nieuw account voor de opslag en onze belangrijke kluis. We ook hebt ingesteld de **-ingeschakeld** markering **$true** en stel in de categorie op AuditEvent (de enige categorie voor sleutel kluis logboekregistratie):


    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

De uitvoer hiervoor bevat:

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0


Dit bevestigt logboekregistratie is nu ingeschakeld voor uw sleutel kluis, informatie aan uw account opslagruimte op te slaan.

U kunt desgewenst ook bewaarbeleid instellen voor uw Logboeken zodat oudere logboeken worden automatisch verwijderd. Bijvoorbeeld, bewaarbeleid verplicht te maken met de vlag **- RetentionEnabled** naar **$true** instellen en **- RetentionInDays** parameter ingesteld op **90** zodat logboeken die ouder zijn dan 90 dagen automatisch worden verwijderd.

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent -RetentionEnabled $true -RetentionInDays 90

Wat wordt vastgelegd:

- Alle geverifieerde REST API aanvragen bent aangemeld, waaronder mislukte aanvragen grond toegangsmachtigingen, systeemfouten of ongeldige aanvragen.
- Bewerkingen op de toets vault zelf, waaronder maken, verwijderen, de instelling belangrijke kluis-beleid, en bijwerken van belangrijke kluis kenmerken, zoals tags.
- Bewerkingen op toetsen en geheimen in de belangrijkste kluis, waaronder maken, wijzigen of verwijderen van deze toetsen of geheimen; bewerkingen zoals teken, controleren, versleutelen, ontsleutelen, laten teruglopen en pak toetsen en krijg geheimen, lijst toetsen en geheimen en hun versies.
- Niet-geverifieerde aanvragen die in een 401 antwoord resulteren. Bijvoorbeeld aanvragen die ik heb een dragertoken of zijn onjuist of verlopen, of een ongeldig token.  


## <a id="access"></a>Toegang tot uw logboeken ##

Belangrijke kluis logboeken worden opgeslagen in de container **inzichten-logboeken-auditevent** in het opslag-account dat u hebt opgegeven. Als alle BLOB's in deze container, typt u:

    Get-AzureStorageBlob -Container 'insights-logs-auditevent' -Context $sa.Context

De uitvoer er iets ongeveer als volgt uit:

**Container Uri: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent**


**Naam**

**----**

**resourceId = / ABONNEMENTEN/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json**

**resourceId = / ABONNEMENTEN/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json**

** resourceId = / ABONNEMENTEN/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json***


Zoals u van deze uitvoer ziet, volgt u de BLOB's een naamgevingsconventie: **resourceId =<ARM resource ID>/y =<year>/m =<month>/d =<day of month>/h =<hour>/m =<minute>/filename.json**

De datum en tijd waarden gebruik UTC.

Omdat het opslag-account kan worden gebruikt voor het verzamelen van Logboeken voor meerdere resources, is de volledige resource-ID in de naam van de blob bijzonder nuttig zijn toegang krijgen tot of downloaden van alleen de BLOB's die u nodig hebt. Maar voordat we dat doet, eerst worden besproken het downloaden van alle BLOB's.

Maak eerst een map als u wilt downloaden van de BLOB's. Bijvoorbeeld:

    New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force

Vervolgens krijgt u een lijst met alle BLOB's:  

    $blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context

Deze lijst tot en met 'Get-AzureStorageBlobContent' om te downloaden van de BLOB's in onze doelmap pipe:

    $blobs | Get-AzureStorageBlobContent -Destination 'C:\Users\username\ContosoKeyVaultLogs'

Wanneer u deze opdracht tweede uitvoert de **/** als scheidingsteken in de namen blob maakt u een volledige mapstructuur onder de doelmap en deze structuur wordt gebruikt voor het downloaden en de BLOB's opslaan als bestanden.

Als u wilt downloaden selectief BLOB's, door jokertekens te gebruiken. Bijvoorbeeld:

- Als u meerdere belangrijke kluizen hebt en u wilt downloaden van Logboeken voor slechts één key kluis, met de naam CONTOSOKEYVAULT3:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3

- Als u meerdere resourcegroepen en u wilt downloaden van Logboeken voor slechts één resourcegroep hebt, gebruikt u `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'

- Als u downloaden van alle de logboeken voor de maand van januari 2016 wilt, gebruikt u `-Blob '*/year=2016/m=01/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'

U bent nu klaar om te kijken wat staat er in de Logboeken starten. Maar voordat u verdergaat naar die, twee meer parameters voor Get-AzureRmDiagnosticSetting die u moet weten:

- Aan de status van diagnostische instellingen voor de resource belangrijke kluis query:`Get-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId`

- Logboekregistratie voor de resource belangrijke kluis uitschakelen:`Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories AuditEvent`


## <a id="interpret"></a>Interpreteren van uw sleutel kluis-Logboeken ##

Afzonderlijke BLOB's worden opgeslagen als tekst zijn opgemaakt als een blob JSON. Dit is een vermelding voorbeeld wordt uitgevoerd `Get-AzureRmKeyVault -VaultName 'contosokeyvault'`:

    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }


De volgende tabel bevat de namen en beschrijvingen.


| Veldnaam        | Beschrijving |
| ------------- |-------------|
| tijd      | Datum en tijd (UTC).|
| resourceId      | Azure resourcemanager Resource-ID. Toets kluis logboekbestanden is dit altijd de sleutel kluis resource-ID.|
| operationName      | De naam van de bewerking, zoals beschreven in de volgende tabel.|
| operationVersion      | Dit is de versie van de REST API aangevraagd door de klant.|
| categorie      | Toets kluis logboekbestanden is AuditEvent de waarde enkel, beschikbaar.|
| resultType      | Resultaat van REST API aanvraag kunt invullen.|
| resultSignature      | HTTP-status.|
| resultDescription     | Beschrijving van de aanvullende informatie over het resultaat, indien beschikbaar.|
| durationMs      | Benodigde tijd om de aanvraag REST API (in milliseconden). Dit is de netwerklatentie niet inbegrepen zodat de tijd die u aan de clientzijde meten mogelijk niet overeenkomen met ditmaal.|
| callerIpAddress      | IP-adres van de client die de aanvraag hebben gedaan.|
| correlationId      | Een optioneel GUID die de client doorgeven kunt als u wilt relateren aan de clientzijde logboeken waaraan de logboekbestanden service aan de clientzijde (toets kluis).|
| identiteit      | De identiteit van de token die bij het maken van de REST API-aanvraag is ingediend. Dit is meestal een "gebruiker", 'service principal' of een combinatie "gebruiker + toepassings-id" zoals in het geval van een aanvraag die resulteert uit een Azure PowerShell-cmdlet.|
| Eigenschappen      | Dit veld wordt verschillende informatie op basis van de bewerking (operationName) bevatten. In de meeste gevallen bevat informatie over client (de useragent tekenreeks doorgegeven door de client), de exacte REST API verzoek URI en HTTP-statuscode. Daarnaast wanneer een object wordt geretourneerd als gevolg van een aanvraag (bijvoorbeeld KeyCreate of VaultGet) bevat ook de sleutel URI (zoals "id"), kluis URI of geheim URI.|




De waarden van de **operationName** -velden zijn in ObjectVerb-indeling. Bijvoorbeeld:

- Alle belangrijke kluis-bewerkingen bevatten de ' kluis`<action>`' opmaken, zoals `VaultGet` en `VaultCreate`.

- Alle belangrijke bewerkingen bevatten de ' sleutel`<action>`' opmaken, zoals `KeySign` en `KeyList`.

- Alle geheime-bewerkingen bevatten de ' geheim`<action>`' opmaken, zoals `SecretGet` en `SecretListVersions`.

De volgende tabel bevat de operationName en de bijbehorende REST API-opdracht.

| operationName        | REST API-opdracht |
| ------------- |-------------|
| Verificatie      | Via Azure Active Directory-eindpunt|
| VaultGet      | [Informatie over een belangrijke kluis](https://msdn.microsoft.com/en-us/library/azure/mt620026.aspx)|
| VaultPut      | [Maken of een belangrijke kluis bijwerken](https://msdn.microsoft.com/en-us/library/azure/mt620025.aspx)|
| VaultDelete      | [Een belangrijke kluis verwijderen](https://msdn.microsoft.com/en-us/library/azure/mt620022.aspx)|
| VaultPatch      | [Een belangrijke kluis bijwerken](https://msdn.microsoft.com/library/azure/mt620025.aspx)|
| VaultList      | [Lijst van alle belangrijke kluizen in een resourcegroep](https://msdn.microsoft.com/en-us/library/azure/mt620027.aspx)|
| KeyCreate      | [Maken van een sleutel](https://msdn.microsoft.com/en-us/library/azure/dn903634.aspx)|
| KeyGet      | [Informatie over een sleutel](https://msdn.microsoft.com/en-us/library/azure/dn878080.aspx)|
| KeyImport      | [Een sleutel importeren in een kluis](https://msdn.microsoft.com/en-us/library/azure/dn903626.aspx)|
| KeyBackup      | [Back-up van een sleutel](https://msdn.microsoft.com/en-us/library/azure/dn878058.aspx).|
| KeyDelete      | [Verwijderen van een sleutel](https://msdn.microsoft.com/en-us/library/azure/dn903611.aspx)|
| KeyRestore      | [Een sleutel herstellen](https://msdn.microsoft.com/en-us/library/azure/dn878106.aspx)|
| KeySign      | [Meld u aan met een sleutel](https://msdn.microsoft.com/en-us/library/azure/dn878096.aspx)|
| KeyVerify      | [Controleer of met een sleutel](https://msdn.microsoft.com/en-us/library/azure/dn878082.aspx)|
| KeyWrap      | [Laten teruglopen van een sleutel](https://msdn.microsoft.com/en-us/library/azure/dn878066.aspx)|
| KeyUnwrap      | [Pak van een sleutel](https://msdn.microsoft.com/en-us/library/azure/dn878079.aspx)|
| KeyEncrypt      | [Versleutelen met een sleutel](https://msdn.microsoft.com/en-us/library/azure/dn878060.aspx)|
| KeyDecrypt      | [Versleutelen met een sleutel](https://msdn.microsoft.com/en-us/library/azure/dn878097.aspx)|
| KeyUpdate      | [Bijwerken van een sleutel](https://msdn.microsoft.com/en-us/library/azure/dn903616.aspx)|
| KeyList      | [Een lijst met de toetsen in een kluis](https://msdn.microsoft.com/en-us/library/azure/dn903629.aspx)|
| KeyListVersions      | [Een lijst met de versies van een sleutel](https://msdn.microsoft.com/en-us/library/azure/dn986822.aspx)|
| SecretSet      | [Een geheim maken](https://msdn.microsoft.com/en-us/library/azure/dn903618.aspx)|
| SecretGet      | [Geheim ophalen](https://msdn.microsoft.com/en-us/library/azure/dn903633.aspx)|
| SecretUpdate      | [Een geheim bijwerken](https://msdn.microsoft.com/en-us/library/azure/dn986818.aspx)|
| SecretDelete      | [Een geheim verwijderen](https://msdn.microsoft.com/en-us/library/azure/dn903613.aspx)|
| SecretList      | [Lijst geheimen in een kluis](https://msdn.microsoft.com/en-us/library/azure/dn903614.aspx)|
| SecretListVersions      | [Lijst-versies van een geheim](https://msdn.microsoft.com/en-us/library/azure/dn986824.aspx)|




## <a id="next"></a>Volgende stappen ##

Zie voor een zelfstudie die gebruikmaakt van Azure-toets kluis in een webtoepassing [Gebruik Azure toets kluis uit een webtoepassing](key-vault-use-from-web-application.md).

Zie [de Azure toets kluis handleiding voor ontwikkelaars](key-vault-developers-guide.md)voor programming verwijzingen.

Zie voor een lijst met Azure PowerShell 1.0-cmdlets voor Azure-toets kluis, [Azure toets kluis Cmdlets](https://msdn.microsoft.com/library/azure/dn868052.aspx).

Lees [hoe u setup toets kluis met complete belangrijke draaiing en controle](key-vault-key-rotation-log-monitoring.md)voor een zelfstudie over log controle met Azure-toets kluis en belangrijke draaiing.
