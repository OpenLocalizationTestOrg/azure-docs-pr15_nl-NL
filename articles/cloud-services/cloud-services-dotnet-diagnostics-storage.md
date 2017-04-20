<properties
    pageTitle="Opslaan en weergave diagnostische gegevens in Azure opslag | Microsoft Azure"
    description="Diagnostische gegevens van Azure-gegevens ophalen op te slaan Azure en te bekijken"
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor="tysonn" />
<tags
    ms.service="cloud-services"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/01/2016"
    ms.author="robb" />

# <a name="store-and-view-diagnostic-data-in-azure-storage"></a>Diagnostische gegevens in Azure Storage Store en weergave

Diagnostische gegevens wordt niet permanent opgeslagen tenzij u deze op de Microsoft Azure opslag emulator of op Azure opslag overbrengen. Eenmaal in opslag, deze kan worden bekeken met een van de verschillende beschikbare hulpmiddelen.

## <a name="specify-a-storage-account"></a>Geef een opslag-account

U opgeven het opslag-account dat u wilt gebruiken in het bestand ServiceConfiguration.cscfg. De accountgegevens wordt gedefinieerd als een verbindingsreeks in een configuratie-instelling. Het volgende voorbeeld ziet de standaard-verbindingsreeks gemaakt voor een nieuw project Cloudservice in Visual Studio:


```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

U kunt deze verbindingsreeks met account-informatie voor een account Azure opslagruimte wijzigen.

Afhankelijk van het type diagnostische gegevens die worden verzameld, diagnostisch hulpprogramma Azure gebruikt de Blob-service of de service van de tabel. De volgende tabel ziet u de gegevensbronnen die worden doorgevoerd en hun notatie.

|Gegevensbron|Opslagindeling|
|---|---|
|Azure Logboeken|Tabel|
|IIS 7.0 Logboeken|BLOB|
|Azure diagnostisch hulpprogramma infrastructuur Logboeken|Tabel|
|Is mislukt logboeken voor het traceren aanvragen|BLOB|
|Gebeurtenislogboeken van Windows|Tabel|
|Prestatie-items|Tabel|
|Crashdumps|BLOB|
|Aangepaste foutenlogboeken|BLOB|

## <a name="transfer-diagnostic-data"></a>Diagnostische gegevens overbrengen

Voor SDK 2,5 en hoger gebruikt, kan het verzoek om over te brengen diagnostische gegevens via het configuratiebestand optreden. U kunt diagnostische gegevens regelmatig zoals opgegeven in de configuratie overbrengen.

Voor SDK 2,4 en vorige die u kunt ook de diagnostische gegevens als via een programma via het configuratiebestand ook overbrengen. Het programma benadering ook kunt u doen op aanvraag overdrachten.


>[AZURE.IMPORTANT] Wanneer u diagnostische gegevens naar een Azure opslag-account overbrengen, worden u kosten voor de opslagbronnen die uw diagnostische gegevens wordt gebruikt.

## <a name="store-diagnostic-data"></a>Diagnostische gegevens op te slaan

Logboekgegevens is opgeslagen in de opslagruimte voor Blob of tabel met de volgende namen:

**Tabellen**

- **WadLogsTable** - Logboeken geschreven in code met behulp van de trace luisteraar ervan af.

- **WADDiagnosticInfrastructureLogsTable** - diagnostische monitor en configuratie gewijzigd.

- **WADDirectoriesTable** – mappen die de diagnostische monitor worden gecontroleerd.  Dit geldt ook voor IIS-logboeken, IIS is mislukt verzoek logboeken en aangepaste mappen.  De locatie van het logboekbestand blob is opgegeven in het veld Container en de naam van de label in het veld RelativePath.  Het veld AbsolutePath bevat de locatie en naam van het bestand zoals deze op de Azure virtuele machine bestaan.

- **WADPerformanceCountersTable** – prestatie-items.

- **WADWindowsEventLogsTable** – gebeurtenislogboeken van Windows.

**BLOB 's**

- **af-besturingselement-container** – bevat (alleen voor SDK 2,4 en vorige) de XML-configuratie-bestanden waarmee wordt bepaald van de Azure diagnostische hulpprogramma's.

- **af-iis-failedreqlogfiles** – bevat informatie van IIS is mislukt aanvragen Logboeken.

- **af-iis-logboekbestanden** – bevat informatie over IIS-logboeken.

- **"aangepaste"** : een aangepaste container op basis van het configureren van de mappen die worden gecontroleerd door de diagnostische monitor.  De naam van deze container blob wordt in WADDirectoriesTable worden opgegeven.

## <a name="tools-to-view-diagnostic-data"></a>Hulpmiddelen voor diagnostische gegevens moeten worden weergegeven
Er zijn verschillende hulpprogramma's beschikbaar zijn voor het weergeven van de gegevens na het overbrengen naar opslag. Bijvoorbeeld:

- Server Explorer in Visual Studio - als u de hulpmiddelen Azure hebt geïnstalleerd voor Microsoft Visual Studio, kunt u het knooppunt Azure-opslag in Server Explorer u alleen-lezen blob en gegevens in een tabel uit uw accounts Azure opslag kunt bekijken. U kunt gegevens weergeven van uw lokale opslag emulator-account en ook uit accounts opslagruimte u hebt gemaakt voor Azure. Zie [surfen en Resources op de opslag beheren met de Server Explorer](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md)voor meer informatie.

- [Microsoft Azure opslag Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is een zelfstandig product-app waarmee u kunt eenvoudig werken met gegevens van Azure-opslag voor Windows, OSX en Linux.

- [Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) bevat Azure diagnostisch hulpprogramma Manager waarmee u kunt bekijken, downloaden en de gegevens van de diagnostische gegevens die worden verzameld door de toepassingen op Azure beheren.


## <a name="next-steps"></a>Volgende stappen

[De stroom in een Cloud Services-toepassing met diagnostisch hulpprogramma Azure aanwijzen](cloud-services-dotnet-diagnostics-trace-flow.md)
