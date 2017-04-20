<properties 
    pageTitle="End-to-End problemen oplossen met Azure opslageenheden en logboekregistratie, AzCopy en bericht Analyzer | Microsoft Azure" 
    description="Een zelfstudie aan te tonen end-to-end problemen oplossen met Azure opslag analyses, AzCopy en Microsoft bericht Analyzer" 
    services="storage" 
    documentationCenter="dotnet" 
    authors="robinsh" 
    manager="carmonm"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/03/2016" 
    ms.author="robinsh"/>

# <a name="end-to-end-troubleshooting-using-azure-storage-metrics-and-logging-azcopy-and-message-analyzer"></a>End-to-End-problemen oplossen met Azure opslageenheden en logboekregistratie, AzCopy en bericht Analyzer 

[AZURE.INCLUDE [storage-selector-portal-e2e-troubleshooting](../../includes/storage-selector-portal-e2e-troubleshooting.md)]

## <a name="overview"></a>Overzicht

Is een belangrijke vaardigheden voor het maken en ondersteunende clienttoepassingen met Microsoft Azure Storage diagnose en probleemoplossing. Vanwege de gedistribueerde aard van een Azure-toepassing, diagnose en probleemoplossing van fouten en prestatieproblemen mogelijk complexer dan in traditionele omgevingen.

In deze zelfstudie wordt gedemonstreerd identificeren client bepaalde fouten die mogelijk invloed hebben op prestaties en deze fouten corrigeren van end-to-end hulpprogramma's van Microsoft en Azure-opslag, met de clienttoepassing optimaliseren. 

Deze zelfstudie bestaat uit een praktische uitleg van een end-to-end voor probleemoplossing scenario. Zie voor een uitgebreide conceptuele handleiding voor het oplossen van Azure opslagtoepassingen, [Monitor, automatisch opsporen, en problemen met Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md). 

## <a name="tools-for-troubleshooting-azure-storage-applications"></a>Hulpprogramma's voor probleemoplossing van Azure Storage-toepassingen

Om op te lossen met Microsoft Azure Storage-clienttoepassingen, kunt u een combinatie van hulpmiddelen voor om te bepalen wanneer er een probleem is opgetreden en wat de oorzaak van het probleem mogelijk. Deze hulpmiddelen zijn:

- **Azure opslag Analytics**. [Azure opslag Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx) biedt de doelstellingen en logboekregistratie voor de opslag van Azure.
    - **Metrische opslaggegevens** wordt bijgehouden transactie maatstaven en capaciteit aan de doelstellingen voor uw account opslag. Aan de doelstellingen gebruikt, kunt u bepalen hoe uw toepassing wordt uitgevoerd op basis van een aantal verschillende eenheden. Zie [Opslagruimte Analytics aan de doelstellingen tabelschema](http://msdn.microsoft.com/library/azure/hh343264.aspx) voor meer informatie over de verschillende typen bijgehouden door opslag Analytics aan de doelstellingen. 

    - **Opslag logboekregistratie** logboeken elk verzoek om een met de services Azure opslagruimte aan een logboek servers. Het logboek kunt u gedetailleerde gegevens voor elke aanvraag, inclusief de bewerking uitgevoerd, de status van de bewerking en latentie informatie bijhouden. Zie [Opslagruimte Analytics Log opmaken](http://msdn.microsoft.com/library/azure/hh343259.aspx) voor meer informatie over de vergaderverzoeken en antwoorden gegevens die door opslag Analytics naar de logboekbestanden geschreven.

> [AZURE.NOTE] Opslag-accounts met een type herhaling van Zone-redundante opslag (ZRS) beschikt niet over de aan de doelstellingen of de mogelijkheid van de logboekregistratie is ingeschakeld op dit moment. 

- **Azure klassieke Portal**. U kunt aan de doelstellingen en logboekregistratie configureren voor uw account opslag in de [Klassieke Azure-Portal](https://manage.windowsazure.com). U kunt ook grafieken en diagrammen hoe uw toepassing wordt uitgevoerd na verloop van tijd weergeven en configureren van melden als uw toepassing anders uitvoert dan verwacht voor een opgegeven waarde. 
    
    Zie [Monitor met een account opslag in de Portal Azure](storage-monitor-storage-account.md) voor informatie over het configureren van de cmdlets voor controle in de klassieke Azure-Portal.

- **AzCopy**. Serverlogboeken aan de voor de opslag van Azure worden opgeslagen als BLOB's, zodat u AzCopy gebruiken kunt om het logboek BLOB's kopiëren naar een lokale map voor analyse met Microsoft bericht Analyzer. Zie [overbrengen van gegevens met het hulpprogramma AzCopy-opdrachtregelopties](storage-use-azcopy.md) voor meer informatie over AzCopy.

- **Microsoft Message Analyzer**. Bericht Analyzer is een functie die verbruikt logboekbestanden en logboekgegevens worden weergegeven in een visuele indeling waarmee u gemakkelijk kunt filteren, zoeken en groep logboekgegevens in handige sets die u gebruiken kunt om fouten en prestatieproblemen te analyseren. Zie [Microsoft bericht Analyzer besturingsomgeving Guide](http://technet.microsoft.com/library/jj649776.aspx) voor meer informatie over bericht Analyzer.

## <a name="about-the-sample-scenario"></a>Over de voorbeeldscenario

Voor deze zelfstudie bestudeert u een scenario waar Azure opslageenheden wordt aangegeven met een lage percentage success rente voor een toepassing die Azure opslagruimte u belt. De laag percentage success tarief meetwaarde (weergegeven als **PercentSuccess** in de klassieke Azure-Portal en in de tabellen aan de doelstellingen) wordt bijgehouden bewerkingen die gevonden, maar die een HTTP-statuscode die groter is dan 299 retourneren. In de logboeken aan de clientzijde opslag, worden deze bewerkingen opgenomen met de transactiestatus **ClientOtherErrors**. Zie voor meer informatie over de meetwaarde lage percentage voltooid, [lage PercentSuccess zien of analytics logboekvermeldingen bewerkingen hebben met de status ClientOtherErrors](storage-monitoring-diagnosing-troubleshooting.md#metrics-show-low-percent-success).

Azure opslagbewerkingen kunnen retourneren HTTP-statuscodes groter is dan 299 als onderdeel van hun normale functionaliteit. Maar deze fouten in sommige gevallen geven aan dat u mogelijk de clienttoepassing voor verbeterde prestaties optimaliseren. 

In dit scenario wordt we een tarief lage percentage succes moeten dan 100% overwegen. U kunt een ander metrische niveau, echter kiezen op basis van uw behoeften voldoet. Het is raadzaam dat tijdens het testen van uw toepassing, u een tolerantie volgens de basislijn voor uw voornaamste prestatiestatistieken ophalen maakt. Bijvoorbeeld, kunt u wellicht bepalen, op basis van testen, dat uw toepassing een consistente percentage success tarief van 90% of 85% nodig hebt. Als u uw gegevens aan de doelstellingen ziet dat de toepassing die is afwijken van datzelfde getal en kunt u de oorzaak van de toename onderzoeken. 

In ons voorbeeld scenario zodra we hebt ingesteld dat het percentage voltooid tarief metrisch is dan 100%, bespreken we de logboeken om te zoeken van de fouten die relateren aan de doelstellingen en deze gebruiken om te bepalen wat het tarief weer lager percentage success veroorzaakt. Bespreken we specifiek fouten in het 400 bereik. Vervolgens gaat we nauwer 404 (niet gevonden)-fouten onderzoek.

### <a name="some-causes-of-400-range-errors"></a>Enkele oorzaken van aanmeldingsfouten 400-bereik

De onderstaande voorbeelden ziet u een voorbeeld van enkele fouten 400-bereik voor aanvragen tegen Azure-blobopslag en hun mogelijke oorzaken. Een van deze fouten, evenals de fouten in het bereik 300 en de 500 bereik, kan bijdragen aan een laag percentage success tarief. 

Houd er rekening mee dat de onderstaande lijsten verre voltooid zijn. Zie [Status en foutcodes](http://msdn.microsoft.com/library/azure/dd179382.aspx) voor meer informatie over algemene Azure Storage fouten en fouten die specifiek zijn voor elk van de storage-services.

**Voorbeelden van de status-Code 404 (niet gevonden)**

Deze gebeurtenis vindt plaats wanneer een lees ten opzichte van een container of blob mislukt omdat het blob of de container niet is gevonden.

- Treedt op als een container of blob is verwijderd door een andere client voordat u dit verzoek. 
- Treedt op als u een API-aanroep waarmee de container of blob nadat u hebt gecontroleerd of dat bestaat gebruikt. De CreateIfNotExists APIs bellen hoofd eerst om te controleren of de container of blob; Als dit niet bestaat nog, een 404-fout als resultaat gegeven en klikt u vervolgens een tweede opslag-oproep wordt uitgevoerd om de container of blob te schrijven.

**Statuscode 409 (Conflict)-voorbeelden**

- Treedt op als u een API maken gebruik te maken van een nieuwe container of blob, zonder eerst aanwezigheid controleren en een container of blob met die naam al bestaat. 
- Treedt op als een container wordt verwijderd en u probeert te maken van een nieuwe container met dezelfde naam voordat het verwijderen betrekking heeft is voltooid.
- Treedt op als u een lease Geef op een container of blob en er al een lease presenteren bestaat.
 
**Statuscode 412 (voorwaarde is mislukt) voorbeelden**

- Deze gebeurtenis vindt plaats wanneer de voorwaarde in de kop van een voorwaardelijke niet is voldaan.
- Deze gebeurtenis vindt plaats wanneer de lease-ID die is opgegeven niet overeenkomen met de lease-ID in de container of blob.

## <a name="generate-log-files-for-analysis"></a>Logboekbestanden voor analyse genereren

In deze zelfstudie gebruiken we bericht Analyzer voor gebruik met drie verschillende typen logboekbestanden, hoewel kunt u werken met een van deze artikelen:

- Het **serverlogboek**, dat wordt gemaakt wanneer u Azure Storage-logboekregistratie inschakelt. Het serverlogboek bevat gegevens over elke bewerking genoemd ten opzichte van een van de Azure Storage-services - blob, wachtrij tabel en bestand. Logboek van de server wordt aangegeven welke bewerking heet en welke statuscode is geretourneerde, evenals andere details over de vergaderverzoeken en antwoorden.
- Het **logboek met .NET-client**, dat wordt gemaakt wanneer u de logboekregistratie aan de clientzijde van binnen uw .NET-toepassing inschakelt. Het clientlogboek bevat gedetailleerde informatie over hoe de client bereidt de aanvraag en ontvangen en verwerkt door het antwoord. 
- Het **http-netwerk doelcellen log**, dat gegevens worden verzameld op HTTP-/ HTTPS vergaderverzoeken en antwoorden gegevens, inclusief voor bewerkingen op Azure Storage. In deze zelfstudie genereert we de netwerktracering via bericht Analyzer.

### <a name="configure-server-side-logging-and-metrics"></a>Logboekregistratie op servers en aan de doelstellingen configureren

Eerst moet we configureren Azure Storage logboekregistratie en aan de doelstellingen, zodat er gegevens uit de clienttoepassing te analyseren. U kunt logboekregistratie en aan de doelstellingen op tal van manieren - via de [Klassieke Azure-Portal](https://manage.windowsazure.com)configureren via PowerShell, of via een programma. Zie [opslageenheden inschakelen en aan de doelstellingen gegevens weergeven](http://msdn.microsoft.com/library/azure/dn782843.aspx) en [vastleggen van opslag inschakelen en logboekgegevens openen](http://msdn.microsoft.com/library/azure/dn782840.aspx) voor meer informatie over het configureren van logboekregistratie en aan de doelstellingen.

**Via de Portal van Azure klassieke**

Volg de instructies op de [Monitor een opslag-account in de Portal Azure](storage-monitor-storage-account.md)configureren logboekregistratie en aan de doelstellingen voor uw opslag-account met behulp van de portal.

> [AZURE.NOTE] Niet is het mogelijk om in te stellen met behulp van de Azure klassieke Portal de minuut doelstellingen. Echter, is het raadzaam dat u deze instellen voor de toepassing van deze zelfstudie en voor wordt onderzocht prestatieproblemen met uw toepassing. Minuut doelstellingen via PowerShell, zoals hieronder of via een programma of via de Portal van Azure-klassiek, kunt u instellen.
>
> Houd er rekening mee dat de klassieke Azure-Portal minuut doelstellingen, alleen per uur aan de doelstellingen kan niet worden weergegeven. 

**Via PowerShell**

Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md)om te beginnen met PowerShell voor Azure.

1. Gebruik de cmdlet [Toevoegen-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) uw Azure gebruikersaccount toevoegen aan het venster PowerShell:

    ```
    Add-AzureAccount
    ```

2. Typ het e-mailadres en wachtwoord in voor uw account in het venster **Meld u aan bij Microsoft Azure** . Azure wordt geverifieerd door de referentiegegevens opslaat en sluit het venster.
3. Het standaardaccount voor de opslag de opslag-account dat u voor de zelfstudie gebruikt door het uitvoeren van deze opdrachten in het venster PowerShell instellen:

    ```
    $SubscriptionName = 'Your subscription name'
    $StorageAccountName = 'yourstorageaccount' 
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName 
    ```

4. Opslag logboekregistratie voor de service Blob inschakelt: 
 
    ```
    Set-AzureStorageServiceLoggingProperty -ServiceType Blob -LoggingOperations Read,Write,Delete -PassThru -RetentionDays 7 -Version 1.0 
    ```
5. Inschakelen opslageenheden voor de service Blob, ervoor te zorgen dat om **- MetricsType** in te stellen `Minute`:

    ```
    Set-AzureStorageServiceMetricsProperty -ServiceType Blob -MetricsType Minute -MetricsLevel ServiceAndApi -PassThru -RetentionDays 7 -Version 1.0 
    ```

### <a name="configure-net-client-side-logging"></a>Logboekregistratie op clients .NET configureren

Als u wilt configureren logboekregistratie aan de clientzijde voor een .NET-toepassing, inschakelen .NET diagnostische hulpprogramma's van de toepassing configuratiebestand (web.config of app.config). Zie [aan de clientzijde logboekregistratie met de bibliotheek .NET opslag Client](http://msdn.microsoft.com/library/azure/dn782839.aspx) en [aan de clientzijde logboekregistratie met de Microsoft Azure opslag SDK for Java](http://msdn.microsoft.com/library/azure/dn782844.aspx) voor meer informatie.

Het logboek aan de clientzijde bevat gedetailleerde informatie over hoe de client bereidt de aanvraag en ontvangen en verwerkt door het antwoord.

De bibliotheek van de Client opslag slaat aan de clientzijde logboekgegevens op de locatie die is opgegeven in van de toepassing configuratiebestand (web.config of app.config). 

### <a name="collect-a-network-trace"></a>Een netwerktracering verzamelen

U kunt bericht Analyzer gebruiken voor het verzamelen van een HTTP-/ HTTPS-netwerktracering terwijl de clienttoepassing wordt uitgevoerd. In het bericht Analyzer wordt [Fiddler](http://www.telerik.com/fiddler) gebruikt op de back-end. Voordat u de netwerktracering verzamelen, wordt u aangeraden Fiddler om vast te leggen versleutelde HTTPS-verkeer te configureren:

1. Installeer [Fiddler](http://www.telerik.com/download/fiddler).
2. Fiddler starten.
2. Selecteer **Extra | Opties voor fiddler**.
3. In het dialoogvenster Opties voor zorgen dat **HTTPS Hiermee vastleggen** en **HTTPS-verkeer ontsleutelen** beide zijn ingeschakeld, zoals hieronder wordt weergegeven.

![Opties voor Fiddler configureren](./media/storage-e2e-troubleshooting-classic-portal/fiddler-options-1.png)

De zelfstudie, verzamelen en een netwerktracering eerst opslaan in bericht Analyzer en een sessie analyse als u wilt analyseren de tracering en de logboeken maken. Voor het verzamelen van een netwerktracering in bericht Analyzer:

1. Selecteer in de bericht-Analyzer, **bestand | Snelle doelcellen | Niet-versleuteld HTTPS**.
2. De tracering wordt onmiddellijk gestart. Selecteer **stoppen** de tracering stoppen, zodat we deze op doelcellen opslagverkeer alleen kunt configureren.
3. Selecteer **bewerken** aan de traceringssessie bewerken.
4. Selecteer de koppeling **configureren** aan de rechterkant van de **Microsoft-Pef-WebProxy** ETW-provider.
5. Klik op het tabblad **Provider** in het dialoogvenster **Advanced Settings** .
6. Geef in het veld **Hostname Filter** de eindpunten van uw opslag, gescheiden door spaties. Bijvoorbeeld, kunt u uw eindpunten als volgt; wijzigen `storagesample` op de naam van uw opslag-account:
    
    ``` 
    storagesample.blob.core.windows.net storagesample.queue.core.windows.net storagesample.table.core.windows.net 
    ```

7. Sluit het dialoogvenster en klik op **Start opnieuw op** om te beginnen met het verzamelen van de tracering met het filter hostname op hun plaats staan, zodat alleen Azure Storage-netwerkverkeer is opgenomen in de trace.

>[AZURE.NOTE] Nadat u klaar bent met het verzamelen van uw netwerktrace, wordt aangeraden dat u de instellingen die u mogelijk hebt gewijzigd in Fiddler in ontsleutelen HTTPS-verkeer terughalen. Schakel de selectievakjes **HTTPS Hiermee vastleggen** en **Ontsleutelen HTTPS-verkeer is toegestaan** in het dialoogvenster Opties voor Fiddler.

Zie [gebruik van de functies voor het traceren van netwerk](http://technet.microsoft.com/library/jj674819.aspx) op Technet voor meer informatie.

## <a name="review-metrics-data-in-the-azure-classic-portal"></a>Aan de doelstellingen gegevens bekijken in de klassieke Azure-Portal

Zodra de toepassing heeft voor een periode is uitgevoerd, kunt u de aan de doelstellingen grafieken die worden weergegeven in de klassieke Azure-Portal om te zien hoe de uitvoering van uw service heeft zijn kunt bekijken. Eerst zullen we de meetwaarde **Success** toevoegen naar de pagina controle:

1. Ga naar het Dashboard voor uw account opslag in de [Klassieke Azure-Portal](https://manage.windowsazure.com)en selecteer vervolgens **Monitor** de controle pagina weergeven.
2. Klik op **Toevoegen aan de doelstellingen** voor het weergeven van het dialoogvenster **Kies aan de doelstellingen** .
3. Schuif omlaag naar de groep **Success Percentage** uitvouwen, selecteer **statistische**, zoals wordt weergegeven in de onderstaande afbeelding. Deze metrisch verzamelt gegevens als percentages slagen van alle Blob bewerkingen.

![Kies aan de doelstellingen](./media/storage-e2e-troubleshooting-classic-portal/choose-metrics-portal-1.png)

Klik in de klassieke Portal Azure nu ziet u **Succespercentage** in het diagram controleren, samen met andere parameters u hebt toegevoegd (maximaal zes kan worden weergegeven in het diagram in één keer). In de volgende afbeelding ziet u dat het tarief weer dat percentage success enigszins dan 100%, dat wil zeggen het scenario dat we vervolgens onderzoeken door te analyseren de logboeken in bericht Analyzer is:

![Aan de doelstellingen grafiek in de portal](./media/storage-e2e-troubleshooting-classic-portal/portal-metrics-chart-1.png)

Zie voor meer informatie over het toevoegen van de doelstellingen aan de pagina controle [hoe: aan de doelstellingen toevoegen aan de tabel aan de doelstellingen](storage-monitor-storage-account.md#how-to-add-metrics-to-the-metrics-table).

> [AZURE.NOTE] Het duurt enige tijd voor uw gegevens aan de doelstellingen worden weergegeven in de klassieke Azure-Portal nadat u metrische opslaggegevens hebt ingeschakeld. Dit komt doordat per uur aan de doelstellingen voor het vorige uur niet in de klassieke Azure-Portal weergegeven worden totdat het huidige uur is verstreken. Minuut aan de doelstellingen worden ook niet weergegeven in de klassieke Azure-Portal. Dus afhankelijk van het wanneer het inschakelen van de doelstellingen, het duurt maximaal twee uur om aan de doelstellingen gegevens te bekijken.

## <a name="use-azcopy-to-copy-server-logs-to-a-local-directory"></a>Gebruik AzCopy serverlogboeken naar een lokale map kopiëren

Azure opslag schrijft server logboekgegevens naar BLOB's, terwijl de doelstellingen naar tabellen zijn geschreven. Log BLOB's zijn beschikbaar in de bekende `$logs` container voor uw account opslag. Log BLOB's zijn met de naam hiërarchisch jaar, maand, dag en uur, zodat u het bereik van de tijd die u wilt onderzoeken gemakkelijk kunt vinden. Bijvoorbeeld, in de `storagesample` -account, de container voor de log BLOB's voor 01-02-2015, van 8 en 9 am, is `https://storagesample.blob.core.windows.net/$logs/blob/2015/01/08/0800`. De afzonderlijke BLOB's in deze container opeenvolgende naam, beginnend met `000000.log`.

U kunt het hulpprogramma voor de AzCopy kunt u deze logboekbestanden serverzijde downloaden naar een locatie van uw keuze op uw lokale computer. U kunt bijvoorbeeld de volgende opdracht gebruiken om te downloaden van de logboekbestanden voor blob bewerkingen die plaatsgevonden op 2 januari 2015 naar de map hebben `C:\Temp\Logs\Server`; Vervang `<storageaccountname>` met de naam van uw account opslag, en `<storageaccountkey>` met uw account toegangstoets:

    AzCopy.exe /Source:http://<storageaccountname>.blob.core.windows.net/$logs /Dest:C:\Temp\Logs\Server /Pattern:"blob/2015/01/02" /SourceKey:<storageaccountkey> /S /V

AzCopy is beschikbaar voor downloaden op de pagina [Downloads van Azure](https://azure.microsoft.com/downloads/) . Zie [overbrengen van gegevens met het hulpprogramma AzCopy-opdrachtregelopties](storage-use-azcopy.md)voor meer informatie over het gebruik van AzCopy.

Zie voor meer informatie over downloaden logboeken aan de serverzijde, [logboekgegevens opslag logboekregistratie downloaden](http://msdn.microsoft.com/library/azure/dn782840.aspx#DownloadingStorageLogginglogdata). 

## <a name="use-microsoft-message-analyzer-to-analyze-log-data"></a>Gebruik Microsoft bericht Analyzer om logboekgegevens te analyseren

Microsoft bericht Analyzer is een hulpprogramma voor het vastleggen, weergeven en analyseren protocol messaging-verkeer is toegestaan, gebeurtenissen en andere berichten-systeem of een toepassing in scenario's voor probleemoplossing en diagnostische gegevens. Bericht Analyzer ook kunt u laden, samenvoegen en analyseren van gegevens uit log en doelcellen bestanden opgeslagen. Zie voor meer informatie over bericht Analyzer [Microsoft bericht Analyzer besturingsomgeving Guide](http://technet.microsoft.com/library/jj649776.aspx).

Activa bevat bericht Analyzer voor Azure-opslag die u helpen bij het analyseren van server, client en netwerk Logboeken. In deze sectie, wordt het gebruik van de hulpmiddelen voor het probleem van lage percentage voltooid in de logboeken opslag besproken.

### <a name="download-and-install-message-analyzer-and-the-azure-storage-assets"></a>Download en installeer bericht Analyzer en de activa Azure-opslag

1. [Bericht Analyzer](http://www.microsoft.com/download/details.aspx?id=44226) downloaden van het Microsoft Download Center en voert u het installatieprogramma.
2. Bericht Analyzer starten.
3. Selecteer in het menu **Extra** **Activa Manager**. Selecteer **Downloads**in het dialoogvenster **Activa Manager** en klik filteren op **Azure Storage**. U ziet de activa van de Azure-opslag, zoals wordt weergegeven in de onderstaande afbeelding.
4. Klik op **Synchroniseren alle weergegeven Items** om te kunnen installeren van de activa van Azure opslag. De beschikbare elementen bevatten:
    - **Azure opslag kleur regels:** Azure opslag kleur regels kunt u definiëren van speciale filters die kleur, tekst en tekenstijlen gebruiken om te markeren van berichten die specifieke informatie in een spoor bevatten.
    - **Azure opslag grafieken:** Azure opslag grafieken zijn vooraf gedefinieerde grafieken die server log grafiekgegevens. Houd er rekening mee dat als u wilt gebruiken Azure Storage grafieken op dit moment, u alleen logboek van de server in het raster analyse laden mogelijk.
    - **Azure opslag Parsers:** De opslag van Azure-parsers parseren de opslag van Azure-client, server en HTTP Logboeken om te kunnen ze worden weergegeven in het raster analyse.
    - **Azure opslag Filters:** Azure opslag filters zijn vooraf gedefinieerde criteria die u gebruiken kunt om query's in uw gegevens in het raster analyse.
    - **Azure opslag weergave indelingen:** Azure opslag weergave indelingen zijn vooraf gedefinieerde kolomopmaak en groeperingen in het raster analyse.
4. Start opnieuw bericht Analyzer nadat u de activa hebt geïnstalleerd.

![Bericht Analyzer activa Manager](./media/storage-e2e-troubleshooting-classic-portal/mma-start-page-1.png)

> [AZURE.NOTE] Installeer alle Azure Storage activa weergegeven voor de toepassing van deze zelfstudie.

### <a name="import-your-log-files-into-message-analyzer"></a>Uw logbestanden importeren in bericht Analyzer

U kunt alle opgeslagen logboekbestanden (aan de clientzijde, aan de clientzijde en netwerk) importeren in één sessie van Microsoft bericht Analyzer voor analyse.

1. Klik in het menu **bestand** in Microsoft bericht Analyzer **Nieuwe sessie**op en klik vervolgens op **Lege sessie**. Voer een naam voor uw analyse-sessie in het dialoogvenster **Nieuwe sessie** . Klik op de knop **bestanden** in het deelvenster **Details van de sessie** . 
1. Als u wilt de trace netwerkgegevens gegenereerd door bericht Analyzer hebt geladen, klikt u op **Bestanden toevoegen**, blader naar de locatie waar u het bestand .matp vanuit uw web traceringssessie opgeslagen, selecteer het bestand .matp en klik op **openen**. 
1. Als u wilt de logboekgegevens serverzijde hebt geladen, klikt u op **Bestanden toevoegen**, blader naar de locatie waar u de logboeken aan de clientzijde hebt gedownload, selecteert u de logboekbestanden voor het tijdsbereik die u wilt analyseren en klik op **openen**. Vervolgens in het deelvenster **Details van de sessie** , stelt u de **Configuratie van tekst** -omlaag voor elk logboekbestand serverzijde op **AzureStorageLog** om ervoor te zorgen dat Microsoft bericht Analyzer kunnen worden geparseerd het logboekbestand correct.
1. Als u wilt de logboekgegevens aan de clientzijde hebt geladen, klikt u op **Bestanden toevoegen**, blader naar de locatie waar u de logboeken aan de clientzijde hebt opgeslagen, selecteert u de logboekbestanden die u wilt analyseren en klik op **openen**. Vervolgens in het deelvenster **Details van de sessie** , stelt u de **Configuratie van tekst** -omlaag voor elk logboekbestand aan de clientzijde op **AzureStorageClientDotNetV4** om ervoor te zorgen dat Microsoft bericht Analyzer kunnen worden geparseerd het logboekbestand correct.
1. Klik in het dialoogvenster **Nieuwe sessie** laden en de logboekgegevens parseren op **Start** . De logboekgegevens wordt weergegeven in het raster bericht Analyzer analyse.

De onderstaande afbeelding ziet een voorbeeld-sessie die is geconfigureerd met de server, client en logbestanden van netwerk doelcellen.

![Bericht Analyzer-sessie configureren](./media/storage-e2e-troubleshooting-classic-portal/configure-mma-session-1.png)

Houd er rekening mee dat bericht Analyzer logboekbestanden in het geheugen laadt. Als u een grote verzameling logboekgegevens hebt, wilt u filteren om te krijgen van de beste prestaties van bericht Analyzer.

Eerst bepalen van de periode die u wilt beoordelen en deze tijdsbestek kleinst mogelijke behouden. U wilt bekijken van een periode van minuten of uren op de meeste in veel gevallen. Importeer de kleinste set logboeken die u kunt aan uw wensen voldoet.

Als u nog een grote hoeveelheid logboekgegevens is, klikt u mogelijk wilt opgeven een sessie datumfilter voor uw gegevens voordat u deze laden. Selecteer in het vak **Sessie Filter** , de knop **bibliotheek** kiezen van een vooraf gedefinieerde filter; bijvoorbeeld kiezen **globale tijd Filter I** uit de opslag van Azure-filters om te filteren op een tijdsinterval. U kunt de filtercriteria opgeven van het eerste en laatste tijdstempel voor het interval dat u wilt zien, klikt u vervolgens bewerken. U kunt ook op een bepaalde statuscode; filteren u kunt bijvoorbeeld alleen logboekvermeldingen waar de statuscode 404 is geladen.

Zie voor meer informatie over het importeren van gegevens in Microsoft bericht Analyzer [Berichtgegevens ophalen](http://technet.microsoft.com/library/dn772437.aspx) op TechNet.

### <a name="use-the-client-request-id-to-correlate-log-file-data"></a>Het verzoek om de client-ID gebruiken om te relateren logboekgegevens

De bibliotheek van Azure opslag Client genereert automatisch een unieke client aanvraag-ID voor elke aanvraag. Deze waarde is geschreven naar het clientlogboek, logboek van de server en de netwerktracering, zodat u deze gebruiken kunt om te relateren van gegevens over alle drie logboeken binnen bericht Analyzer. Zie [Client aanvraag-ID](storage-monitoring-diagnosing-troubleshooting.md#client-request-id) voor meer informatie over het clientverzoek-id.

De secties hierna wordt uitgelegd hoe u de lay-van een vooraf geconfigureerde en aangepaste outweergaven gebruiken om te relateren en gegevens groeperen op basis van de client aanvraag-ID.

### <a name="select-a-view-layout-to-display-in-the-analysis-grid"></a>Selecteer een weergave-indeling om weer te geven in de analyse-raster

De activa opslag voor bericht Analyzer hebben Azure opslag weergave indelingen, die vooraf geconfigureerde weergaven die u gebruiken kunt om uw gegevens met nuttige groeperingen en kolommen voor verschillende scenario's weer te geven. U kunt ook aangepaste weergave-indelingen maken en deze opslaan voor hergebruik. 

De onderstaande afbeelding ziet het menu **Weergave-indeling** , verkrijgbaar via de **Weergave indeling** selecteren in het lint van de werkbalk. De weergave-indelingen voor Azure opslag zijn gegroepeerd onder het knooppunt **Azure-opslag** in het menu. U kunt zoeken naar `Azure Storage` in het zoekvak om te filteren op Azure Storage indelingen alleen bekijken. U kunt ook het sterretje bij een weergave-indeling naar kunt u een favoriete en weer te geven aan de bovenkant van het menu selecteren.

![Weergavemenu-indeling](./media/storage-e2e-troubleshooting-classic-portal/view-layout-menu.png)

Selecteer allereerst **gegroepeerd per ClientRequestID en Module**. Deze groepen weergeven indeling Meld u aan gegevens uit alle drie logboeken eerst op verzoek om de client-ID, vervolgens op bron logboekbestand (of **Module** in bericht Analyzer). Met deze weergave, kunt u inzoomen op een bepaalde client aanvraag-ID en Zie gegevens uit alle drie logboekbestanden voor die clientaanvraag-id.

De afbeelding hieronder ziet u deze weergave-indeling toegepast op de voorbeeldgegevens log wordt weergegeven, met een subset van kolommen die worden weergegeven. U kunt zien dat voor een bepaalde client aanvraag-ID, het raster analyse gegevens worden weergegeven uit het clientlogboek met, server log en netwerktracering.

![Lay-out van Azure opslag weergeven](./media/storage-e2e-troubleshooting-classic-portal/view-layout-client-request-id-module.png)

>[AZURE.NOTE] Verschillende logboekbestanden hebben verschillende kolommen, zodat wanneer u gegevens uit meerdere logbestanden worden weergegeven in het raster analyse, aantal kolommen is mogelijk geen gegevens voor een bepaalde rij. Bijvoorbeeld, in de bovenstaande afbeelding weergeven de client log rijen niet alle gegevens voor de **tijdstempel**, **TimeElapsed**, **bron**en **bestemming** kolommen, omdat deze kolommen niet in het clientlogboek, maar bestaan in het netwerktracering. Daarnaast tijdstempel gegevens uit het logboek van de server in de kolom **tijdstempel** weergegeven, maar geen gegevens worden weergegeven voor de kolommen **TimeElapsed**, **bron**en **bestemming** , die geen deel van het logboek van de server uitmaken. 

Naast de opslag van Azure weergave-indelingen, kunt u ook definiëren en slaat uw eigen weergave-indelingen. U kunt andere gewenste velden voor het groeperen van gegevens selecteren en de groepering als onderdeel van uw aangepaste indeling ook opslaan. 

### <a name="apply-color-rules-to-the-analysis-grid"></a>Kleur regels toepassen op de analyse-raster

De activa opslag ook kleur regels, zoals bieden een visuele betekent dat er verschillende soorten fouten in het raster analyse identificeren. De vooraf gedefinieerde kleur regels toepassen op HTTP-fouten, zodat ze worden alleen voor de server-logboeken en netwerk tracering weergegeven.

Als u wilt kleur regels toepassen, selecteert u **Kleur regels** in het lint van de werkbalk. Hier ziet u de regels van de Azure Storage kleur in het menu. Selecteer voor de zelfstudie **Client fouten (StatusCode tussen 400 en 499)**, zoals wordt weergegeven in de onderstaande afbeelding.

![Lay-out van Azure opslag weergeven](./media/storage-e2e-troubleshooting-classic-portal/color-rules-menu.png)

Naast de regels van Azure Storage kleur, kunt u ook definiëren en uw eigen kleur-regels opslaan.

### <a name="group-and-filter-log-data-to-find-400-range-errors"></a>Gegevens in de groep en filter de log 400-bereik fouten zoeken

Vervolgens we groeperen en de logboekgegevens om te zoeken van alle fouten in het 400 bereik filteren.

1. Zoek de **StatusCode** -kolom in het raster analyse, met de rechtermuisknop op de kolomkop en selecteer **groep**.
2. Volgende, groep op de kolom **ClientRequestId** . U ziet dat de gegevens in het raster analyse nu is georganiseerd door statuscode en client aanvraag-ID.
1. Het venster van de functie Filter weergegeven als dit nog niet wordt weergegeven. Selecteer op het lint van de werkbalk, **Hulpprogramma Windows**, klikt u vervolgens **Weergavefilter**.
2. Het om logboekgegevens te filteren om alleen 400-bereik fouten weer te geven, de volgende filtercriteria toevoegen aan het venster **Filter** en klikt u op **toepassen**:

        (AzureStorageLog.StatusCode >= 400 && AzureStorageLog.StatusCode <=499) || (HTTP.StatusCode >= 400 && HTTP.StatusCode <= 499)

De onderstaande afbeelding ziet de resultaten van deze groeperen en filteren. Het veld **ClientRequestID** onder de groepering voor statuscode 409 uitvouwen, bijvoorbeeld, ziet u een bewerking waardoor er die statuscode.

![Lay-out van Azure opslag weergeven](./media/storage-e2e-troubleshooting-classic-portal/400-range-errors1.png)

Na het toepassen van dit filter, ziet u dat rijen uit het clientlogboek niet worden opgenomen, als de client log is niet inbegrepen bij een kolom **StatusCode** . Allereerst we wordt de server en logboeken voor het traceren van netwerk om te zoeken 404-fouten controleren en vervolgens we keert terug naar het clientlogboek om te bekijken van de clientbewerkingen die onder leiding van een hierop toe.

>[AZURE.NOTE] U kunt filteren op de kolom **StatusCode** en gegevens uit alle drie logboeken, met inbegrip van het clientlogboek met, als u een expressie toevoegen aan het filter dat logboekvermeldingen waar de statuscode null is bevat nog steeds weergeven. Als u wilt samenstellen deze filterexpressie, gebruiken:
>
> <code>&#42;StatusCode >= 400 or !&#42;StatusCode</code> 
>
> Dit filter retourneert alle rijen van de client log en alleen de rijen van het serverlogboek en HTTP-logboek waar de statuscode groter is dan 400. Als u deze naar de weergave-indeling gegroepeerd op verzoek om de client-ID en de module hebt toegepast, kunt u zoeken of schuif door de logboekvermeldingen om te zoeken die waar alle drie logboeken worden aangegeven.   

### <a name="filter-log-data-to-find-404-errors"></a>Filter logboekgegevens 404-fouten zoeken

De activa opslag bevatten vooraf gedefinieerde filters die u gebruiken kunt om te beperken logboekgegevens om de fouten of trends die u zoekt. Vervolgens past we twee vooraf gedefinieerde filters: dat de server en logboeken voor het traceren van netwerk voor 404-fouten worden gefilterd en een die de gegevens op een bepaald tijdsinterval bereik worden gefilterd.

1. Het venster van de functie Filter weergegeven als dit nog niet wordt weergegeven. Selecteer op het lint van de werkbalk, **Hulpprogramma Windows**, klikt u vervolgens **Weergavefilter**.
2. Klik in het venster weergavefilter **bibliotheek**selecteren en zoek op `Azure Storage` filters voor de opslag van Azure zoeken. Selecteer het filter voor **404 (niet gevonden) berichten in alle logboeken**.
3. Het menu **bibliotheek** opnieuw, weergeven en zoek en selecteer het **Filter voor globale tijd**.
4. Bewerk de tijdstempels weergegeven in het filter en het bereik dat u wilt bekijken. Hierdoor kunt beperken door het bereik van gegevens te analyseren.
5. Het filter moet vergelijkbaar met het onderstaande voorbeeld worden weergegeven. Klik op **toepassen** om het filter toepassen op het raster analyse.

        ((AzureStorageLog.StatusCode == 404 || HTTP.StatusCode == 404)) And 
        (#Timestamp >= 2014-10-20T16:36:38 and #Timestamp <= 2014-10-20T16:36:39)

![Lay-out van Azure opslag weergeven](./media/storage-e2e-troubleshooting-classic-portal/404-filtered-errors1.png)

### <a name="analyze-your-log-data"></a>Uw gegevens analyseren

Nu dat u hebt gegroepeerd en uw gegevens gefilterd, kunt u de details van afzonderlijke aanvragen die gegenereerd 404-fouten controleren. In de huidige weergave-indeling, worden de gegevens zijn gegroepeerd client aanvraag-ID, klikt u vervolgens op log bron. Aangezien we op aanvragen waarvan het veld StatusCode 404 bevat filtert, zien we wordt alleen de server en de trace netwerkgegevens, niet de logboekgegevens van client.

De onderstaande afbeelding ziet een specifiek verzoek waar een bewerking Blob krijgen een 404 overgedragen omdat de blob niet bestaat. Houd er rekening mee dat sommige kolommen zijn verwijderd uit de weergave Normaal om de relevante gegevens weer te geven.

![Gefilterde Server en logboeken voor het traceren van netwerk](./media/storage-e2e-troubleshooting-classic-portal/server-filtered-404-error.png)

Vervolgens gaat we dit verzoek om de client-ID met de logboekgegevens van de client om te zien welke acties de client ben wanneer de fout is er gebeurd met relateren. U kunt een nieuwe analyse rasterweergave voor deze sessie om weer te geven van de client logboekgegevens, dat wordt geopend in een tweede tabblad weergeven:

1. De waarde van het veld **ClientRequestId** eerst naar het Klembord kopiëren. U kunt dit doen door een rij selecteren, zoeken naar het veld **ClientRequestId** , met de rechtermuisknop op de waarde data en **kopie 'ClientRequestId'**te kiezen. 
1. Klik op het lint van de werkbalk, selecteert u **Nieuwe Viewer**, en selecteer vervolgens **Analyse raster** om een nieuw tabblad te openen. Het tabblad Nieuw worden alle gegevens in uw logboekbestanden, zonder groeperen, filteren of kleur regels. 
2. Klik op het lint van de werkbalk, selecteert u **Weergave-indeling**en selecteer **Alle kolommen voor .NET-Client** onder de sectie **Azure opslag** . Deze weergave-indeling worden gegevens weergegeven van de client log, evenals de server en netwerk doelcellen Logboeken. Standaard is gesorteerd op de kolom **MessageNumber** .
3. De clientlogboek vervolgens zoeken voor de client aanvraag-ID. Klik op het lint van de werkbalk, selecteer **Berichten zoeken**en geeft u een aangepast filter op de client aanvraag-ID in het veld **Zoeken** . Gebruik de volgende syntaxis voor het filter, uw eigen client aanvraag-ID opgeven:

        *ClientRequestId == "398bac41-7725-484b-8a69-2a9e48fc669a"

Bericht Analyzer zoekt en selecteert u het eerste vermelding waar de zoekcriteria komt overeen met de client aanvraag-ID. In het clientlogboek, zijn er verschillende posten voor elke client aanvraag-ID, zodat u kunt om ze te groeperen op het veld **ClientRequestId** zich eenvoudiger kunt allemaal tegelijk bekijken. De onderstaande afbeelding ziet u alle berichten in het clientlogboek voor de opgegeven client aanvraag-ID. 

![Client logboek met fouten in 404](./media/storage-e2e-troubleshooting-classic-portal/client-log-analysis-grid1.png)

Met de gegevens die in de weergave-indelingen in de volgende twee tabbladen worden weergegeven, kunt u de aanvraaggegevens om te bepalen wat de fout kan veroorzaakt analyseren. U kunt ook aanvragen die worden voorafgegaan dit item om te zien als een vorige gebeurtenis mogelijk hebben geleid tot de 404-fout bekijken. U kunt bijvoorbeeld de client logboekvermeldingen voorafgaand aan de dit verzoek cliënt-ID te bepalen of de blob is verwijderd, of als de fout is vanwege de clienttoepassing een API CreateIfNotExists bellen op een container of blob bekijken. In het clientlogboek met vindt u van de blob adres in het veld **Beschrijving** . in de server en logboeken voor het traceren van netwerk, is deze informatie wordt weergegeven in het veld **Overzicht** .

Nadat u het adres van de blob die hebben de 404-fout opgeleverd kent, kunt u verder onderzoeken. Als u de vermeldingen voor andere berichten die zijn gekoppeld aan bewerkingen op de dezelfde blob zoekt, kunt u controleren of de client eerder de entity verwijderd. 

## <a name="analyze-other-types-of-storage-errors"></a>Andere soorten opslag fouten analyseren

Nu dat u vertrouwd te raken bent met bericht Analyzer logboekgegevens te analyseren, kunt u andere soorten fouten met view analyseren indelingen, kleur regels en zoeken/filteren. De onderstaande tabellen bevat enkele problemen die kunnen optreden en de filtercriteria kunt u deze te vinden. Zie voor meer informatie over het bouwen van filters en de bericht-Analyzer taal filteren, [Berichtgegevens filteren](http://technet.microsoft.com/library/jj819365.aspx).

|    Om te kunnen onderzoeken...                                                                                               |    Gebruik de filterexpressie...                                                                                                                                                                                                                                        |    Expressie is van toepassing op Log (Client, Server, netwerk, alle)    |
|------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------|
|    Onverwachte vertragingen in het bericht is bezorgd op een wachtrij                                                              |    AzureStorageClientDotNetV4.Description bevat "Bezig met opnieuw proberen is bewerking mislukt."                                                                                                                                                                                |    Client                                                        |
|    HTTP-stijging in PercentThrottlingError                                                                       |    HTTP. Response.StatusCode == 500 & #124; & #124; HTTP. Response.StatusCode == 503                                                                                                                                                                                          |    Netwerk                                                       |
|    Vergroten in PercentTimeoutError                                                                               |    HTTP. Response.StatusCode == 500                                                                                                                                                                                                                             |    Netwerk                                                       |
|    Vergroten in PercentTimeoutError (alle)                                                                         |    * StatusCode 500 ==                                                                                                                                                                                                                                          |    Alle                                                           |
|    Vergroten in PercentNetworkError                                                                               |    AzureStorageClientDotNetV4.EventLogEntry.Level < 2                                                                                                                                                                                                          |    Client                                                        |
|    HTTP 403 (verboden) berichten                                                                                 |    HTTP. Response.StatusCode == 403                                                                                                                                                                                                                             |    Netwerk                                                       |
|    HTTP 404 (niet gevonden) berichten                                                                                 |    HTTP. Response.StatusCode == 404                                                                                                                                                                                                                             |    Netwerk                                                       |
|    404 (alle)                                                                                                     |    * StatusCode == 404                                                                                                                                                                                                                                          |    Alle                                                           |
|    Access handtekening (SA's) autorisatie probleem gedeeld                                                             |    AzureStorageLog.RequestStatus == "SASAuthorizationError"                                                                                                                                                                                                     |    Netwerk                                                       |
|    HTTP 409 (Conflict) berichten                                                                                  |    HTTP. Response.StatusCode == 409                                                                                                                                                                                                                             |    Netwerk                                                       |
|    409 (alle)                                                                                                     |    * StatusCode == 409                                                                                                                                                                                                                                          |    Alle                                                           |
|    Laag PercentSuccess of analytics logboekvermeldingen hebben bewerkingen met de status van ClientOtherErrors    |    AzureStorageLog.RequestStatus == "ClientOtherError"                                                                                                                                                                                                         |    Server                                                        |
|    Nagle waarschuwing                                                                                               |    ((AzureStorageLog.EndToEndLatencyMS-AzureStorageLog.ServerLatencyMS) > (AzureStorageLog.ServerLatencyMS * 1,5)) en (AzureStorageLog.RequestPacketSize < 1460) en (AzureStorageLog.EndToEndLatencyMS - AzureStorageLog.ServerLatencyMS > = 200)        |    Server                                                        |
|    Bereik van de tijd in Logboeken aan de Server en netwerk                                                                    |    #Tijdstempel > = 2014-10-20T16:36:38 en #Timestamp < = 2014-10-20T16:36:39                                                                                                                                                                                     |    Server, netwerk                                               |
|    Bereik van de tijd in Logboeken aan de Server                                                                                |    AzureStorageLog.Timestamp > = 2014-10-20T16:36:38 en AzureStorageLog.Timestamp < = 2014-10-20T16:36:39                                                                                                                                                     |    Server                                                        |


## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie over het oplossen van problemen end-to-end-scenario's in Azure Storage:

- [Bewaken, diagnosticeren en problemen met Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md)
- [Opslag Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx)
- [Monitor met een account opslag in de Portal van Azure](storage-monitor-storage-account.md)
- [Gegevens met het hulpprogramma voor de opdrachtregel AzCopy overbrengen](storage-use-azcopy.md)
- [Handleiding voor het besturingssysteem van Microsoft Message-Analyzer](http://technet.microsoft.com/library/jj649776.aspx)
 
 
