<properties
    pageTitle="Met behulp van blobopslag voor IIS en tabel opslag voor gebeurtenissen | Microsoft Azure"
    description="Log Analytics kunt lezen de logboeken voor Azure services die diagnostische gegevens met tabelopslag schrijven of IIS-logboeken naar blob storage geschreven."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="banders"/>


# <a name="using-blob-storage-for-iis-and-table-storage-for-events"></a>Gebruik voor IIS-blobopslag en tabelopslag voor gebeurtenissen

Log Analytics kunt vinden in de logboeken voor de volgende services die diagnostische gegevens met tabelopslag schrijven of IIS-logboeken naar blob storage geschreven:

+ Service stof clusters (Preview)
+ Virtuele Machines
+ Web/werknemer rollen

Voordat Log Analytics van gegevens voor deze bronnen verzamelen kunt, moet Azure diagnostische hulpprogramma's zijn ingeschakeld.

Als diagnostische hulpprogramma's zijn ingeschakeld, kunt u de Azure-portal of PowerShell Log Analytics voor het verzamelen van de logboeken configureren.

Azure diagnostische hulpprogramma's is een Azure uitbreiding waarmee u kunt de diagnostische gegevens verzamelen van een werknemer rol, Webrol of VM uitgevoerd in Azure wordt aangegeven. De gegevens worden opgeslagen in een Azure opslag-account en vervolgens kan worden verzameld door Log Analytics.

Voor Log Analytics deze diagnostisch hulpprogramma Azure-logboeken verzamelen, moet de logboeken op de volgende locaties:

| Logboektype | Resourcetype | Locatie |
| -------- | ------------- | -------- |
| IIS-logboeken | Virtuele Machines <br> Web rollen <br> Werknemer rollen | af-iis-logboekbestanden (Blob Storage) |
| Syslog | Virtuele Machines | LinuxsyslogVer2v0 (opslag van een tabel) |
| Service stof operationele gebeurtenissen | Service stof knooppunten | WADServiceFabricSystemEventTable |
| Service stof betrouwbare acteur gebeurtenissen | Service stof knooppunten | WADServiceFabricReliableActorEventTable |
| Service stof betrouwbare servicegebeurtenissen | Service stof knooppunten | WADServiceFabricReliableServiceEventTable |
| Gebeurtenislogboeken van Windows | Service stof knooppunten <br> Virtuele Machines <br> Web rollen <br> Werknemer rollen | WADWindowsEventLogsTable (Table Storage) |
| Windows ETW Logboeken | Service stof knooppunten <br> Virtuele Machines <br> Web rollen <br> Werknemer rollen | WADETWEventTable (Table Storage) |

>[AZURE.NOTE] IIS-logboeken van Azure-Websites worden momenteel niet ondersteund.

Voor virtuele machines hebt u de optie van het installeren van de [Log Analytics-agent](log-analytics-azure-vm-extension.md) in uw VM extra inzichten inschakelen. U kunt niet alleen om IIS-logboeken en gebeurtenislogboeken te analyseren, aanvullende analyse, zoals het bijhouden van wijzigingen configuratie, SQL assessment en update assessment uitvoeren.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a>Azure diagnostische gegevens in een virtuele machine inschakelen voor gebeurtenislogboek en IIS Meld u aan siteverzamelingen

Gebruik de volgende procedure om in te schakelen Azure diagnostische gegevens in een virtuele machine voor gebeurtenislogboek en IIS verzamelen met behulp van de portal van Microsoft Azure.

### <a name="to-enable-azure-diagnostics-in-a-virtual-machine-with-the-azure-portal"></a>Azure diagnostische gegevens in een virtuele machine in de portal van Azure inschakelen

1. Installeer de VM-Agent wanneer u een virtuele machine maakt. Als de virtuele machine al bestaat, controleert u of dat de VM-Agent al is geïnstalleerd.
    - Klik in de portal Azure Navigeer naar de virtuele machine **Optionele configuratie**en klik vervolgens **diagnostische hulpprogramma's** en selecteer **Aanwezigheidsstatus** instellen op **op**.

    Na voltooiing heeft de VM de extensie diagnostisch hulpprogramma Azure geïnstalleerd en uit te voeren. Dit toestel is verantwoordelijk voor het verzamelen van uw gegevens diagnostische gegevens.

2. Controle inschakelen en configureren van logboekregistratie op een bestaande VM. Diagnostische hulpprogramma's op het niveau van de VM, kunt u ook weer inschakelen. Als u wilt inschakelen diagnostische hulpprogramma's en klikt u vervolgens logboekregistratie configureren, moet u de volgende stappen uitvoeren:
    1. Selecteer de VM.
    2. Klik op **controle**.
    3. Klik op **diagnose**.
    4. De **Status** instellen in **op**aan.
    5. Selecteer elke log diagnostische hulpprogramma's die u wilt verzamelen.
    7. Klik op **OK**.

U kunt nog nauwkeuriger een de gebeurtenissen die zijn geschreven met Azure Storage opgeven via Azure PowerShell. Raadpleeg voor het [verzamelen van gegevens met behulp van Azure-tabelopslag of IIS-logboeken naar blob geschreven geschreven diagnostische hulpprogramma's](log-analytics-azure-storage-json.md).


## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a>Azure diagnostische gegevens in een Webrol voor IIS-logboeken en gebeurtenis verzameling inschakelen

Raadpleeg [Hoe naar inschakelen diagnostische gegevens in een Cloudservice](../cloud-services/cloud-services-dotnet-diagnostics.md) voor algemene stappen over het inschakelen van Azure diagnostische gegevens. De onderstaande instructies deze gegevens gebruiken en aanpassen voor gebruik met Log Analytics.

Met Azure diagnostisch hulpprogramma dat is ingeschakeld:

- IIS-logboeken worden standaard opgeslagen met logboekgegevens overgebracht met het interval dat scheduledTransferPeriod doorverbinden.
- Gebeurtenislogboeken van Windows niet al dan niet standaard worden overgedragen.

### <a name="to-enable-diagnostics"></a>Diagnostische hulpprogramma's inschakelen

Windows-gebeurtenislogboeken inschakelen of wijzigen van de scheduledTransferPeriod, configureren Azure diagnostische gegevens met de configuratie van het XML-bestand (diagnostics.wadcfg), zoals in [stap 4: het configuratiebestand diagnostische hulpprogramma's maken en installeren van de extensie](../cloud-services/cloud-services-dotnet-diagnostics.md)

Het volgende voorbeeld-configuratiebestand berekent de logboeken toepassings- en IIS-logboeken en alle gebeurtenissen:

```
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant to Web roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>
```

Zorg ervoor dat uw ConfigurationSettings Hiermee geeft u een opslag-account, zoals in het volgende voorbeeld:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

De **accountnaam** en **AccountKey** waarden zijn gevonden in de Azure portal in het dashboard van het account opslag, onder toegangstoetsen beheren. Het protocol voor de verbindingsreeks moet **https**.

Zodra de bijgewerkte diagnostische configuratie wordt toegepast op uw cloudservice en het wegschrijven van diagnostische gegevens met Azure Storage, klik bent u klaar voor het configureren van Log Analytics.


## <a name="use-the-azure-portal-to-collect-logs-from-azure-storage"></a>Gebruik de Azure portal logboeken verzamelen uit de opslag van Azure

U kunt de portal van Azure Log Analytics te verzamelen de logboeken voor de volgende Azure services configureren:

+ Service stof clusters
+ Virtuele Machines
+ Web/werknemer rollen

Klik in de portal Azure Ga naar uw Log Analytics-werkruimte en de volgende taken uitvoeren:

1. Klik op *opslag accounts logboeken*
2. Klik op de taak *toevoegen*
3. Selecteer het account opslagruimte met de diagnostische logboeken
  - Dit account kunt werken met een klassieke opslag-account of een account van Azure resourcemanager opslag
4. Selecteer het gegevenstype dat u wilt logboeken voor verzamelen
  - De opties zijn IIS-logboeken; Gebeurtenissen. Syslog (Linux); ETW Logboeken; Service stof gebeurtenissen
5. De waarde voor de bron wordt automatisch ingevuld op basis van het gegevenstype en kan niet worden gewijzigd
6. Klik op OK om de configuratie opslaan

Herhaal stappen 2 tot en met 6 voor de gegevenstypen die u Log Analytics wilt voor het verzamelen en extra opslagruimte accounts.

Ongeveer 30 minuten bent u gegevens uit de opslag-account in Log Analytics zien. U ziet alleen de gegevens die is geschreven naar opslag nadat de configuratie is toegepast. Log Analytics leest niet de bestaande gegevens uit het opslag-account.

>[AZURE.NOTE] De portal valideert niet dat de bron in de opslagruimte-account bestaat of als nieuwe gegevens worden geschreven.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a>Azure diagnostische gegevens in een virtuele machine inschakelen voor gebeurtenislogboek en IIS Meld u aan siteverzameling via PowerShell

Voer de stappen in [Configureren Log Analytics indexeren Azure diagnostisch hulpprogramma](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) PowerShell gebruiken om te lezen van Azure diagnostische hulpprogramma's die zijn geschreven met tabelopslag.

U kunt nog nauwkeuriger een de gebeurtenissen die zijn geschreven met Azure Storage opgeven via Azure PowerShell.
Zie [Diagnostisch hulpprogramma van inschakelen in Azure virtuele Machines](../virtual-machines-dotnet-diagnostics.md)voor meer informatie.

U kunt inschakelen en Azure diagnostisch hulpprogramma voor het gebruik van de volgende PowerShell-script bijwerken.
U kunt dit script ook gebruiken met een aangepaste logboekregistratie-configuratie.
Wijzig het script zodat de opslag-account, de servicenaam van de en de naam van de virtuele machine instellen.
Het script cmdlets voor klassieke virtuele machines wordt gebruikt.

Controleer in het onderstaande voorbeeld van de script, kopieer deze aanpassen om, de steekproef opslaan als een PowerShell-script-bestand en vervolgens het script uitvoeren.

```
    #Connect to Azure
    Add-AzureAccount

    # settings to change:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert to config format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzureStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a>Volgende stappen

- [Gebruik JSON-bestanden in-blobopslag](log-analytics-azure-storage-json.md) lezen van de logboekbestanden van Azure services die schrijven diagnostische gegevens met blob storage in JSON-indeling.
- [Oplossingen inschakelen](log-analytics-add-solutions.md) voor bieden inzicht in de gegevens.
- [Gebruik zoekopdrachten gaat uitvoeren](log-analytics-log-searches.md) om de gegevens te analyseren.
