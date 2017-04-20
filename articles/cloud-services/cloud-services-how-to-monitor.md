<properties 
    pageTitle="Het controleren van een cloudservice | Microsoft Azure" 
    description="Informatie over het controleren van cloudservices met behulp van de Azure klassieke portal." 
    services="cloud-services" 
    documentationCenter="" 
    authors="rboucher" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="robb"/>


# <a name="how-to-monitor-cloud-services"></a>Het controleren van Cloudservices

[AZURE.INCLUDE [disclaimer](../../includes/disclaimer.md)]

U kunt controleren `key` prestatiegegevens voor de cloudservices in de portal van Azure klassieke. U kunt instellen dat het niveau van de minimale en uitgebreide voor elke service rol cmdlets voor controle en de controle wordt weergegeven kunt aanpassen. Uitgebreide controlegegevens wordt opgeslagen in een opslag-account, die toegankelijk is buiten de portal. 

Controle wordt weergegeven in de portal van Azure klassieke kunnen uitgebreid worden geconfigureerd. U kunt ervoor kiezen de cijfers die u wilt controleren in de lijst aan de doelstellingen op de pagina **Beeldscherm** en kunt u kiezen welke maatstelsel uitzetten in aan de doelstellingen grafieken op de pagina **Beeldscherm** en het dashboard. 

## <a name="concepts"></a>Concepten

Standaard is minimale cmdlets voor controle opgegeven voor een nieuwe cloudservice met behulp van prestatiemeteritems uit het hostbesturingssysteem voor de rollen-exemplaren (virtuele machines) verzameld. De minimale aan de doelstellingen zijn beperkt tot CPU Percentage, gegevens In gegevens af, schijfdoorvoer lezen en schrijven schijfdoorvoer. U configureert uitgebreide controle, kunt u extra statistieken op basis van de prestatiegegevens in de virtuele machines (rol exemplaren) ontvangen. De doelstellingen van de uitgebreide inschakelen nabij analyse van problemen die tijdens de bewerkingen van toepassingen optreden.

Standaard wordt prestatiemeteritemgegevens van exemplaren van de rol partijen en overgebracht van het exemplaar van de rol om de 3 minuten. Wanneer u de uitgebreide controle inschakelt, wordt de itemgegevens onbewerkte prestaties voor elk exemplaar van de rol en alle werkstroomexemplaren rol voor elke rol samengevoegd met tussenpozen van 5 minuten, 1 uur en 12 uur. De samengevoegde gegevens wordt na 10 dagen verwijderd.

Nadat u de uitgebreide controle hebt ingeschakeld, wordt de samengevoegde controlegegevens wordt opgeslagen in tabellen in uw account opslag. Als u wilt inschakelen voor een rol voor uitgebreide controle, moet u een verbindingsreeks diagnostische hulpprogramma's die verwijzen naar de opslag-account configureren. U kunt verschillende opslag accounts gebruiken voor verschillende rollen.

Houd er rekening mee dat uw opslagruimte kosten in verband met gegevensopslag, overdracht van gegevens en opslag transacties als u een uitgebreide controle inschakelen neemt. Minimale monitoring, hoeft niet een opslag-account. De gegevens voor de criteria die worden weergegeven op het niveau van de minimale controle worden niet opgeslagen in uw account opslag, zelfs als u het niveau van de controle op uitgebreide instellen.


## <a name="how-to-configure-monitoring-for-cloud-services"></a>Hoe u: monitoring voor cloudservices configureren

Gebruik de volgende procedures voor het configureren van uitgebreide of minimale cmdlets voor controle in de portal van Azure klassieke. 

### <a name="before-you-begin"></a>Voordat u begint

- Maak een account opslag om op te slaan van de controlegegevens. U kunt verschillende opslag accounts gebruiken voor verschillende rollen. Zie voor meer informatie help voor **Opslag-Accounts**, of kijk [hoe u een opslag-Account maken](/manage/services/storage/how-to-create-a-storage-account/).

- Diagnostisch hulpprogramma Azure inschakelen voor uw rollen cloud-service. Zie [Diagnostisch hulpprogramma voor Cloudservices configureren](https://msdn.microsoft.com/library/azure/dn186185.aspx#BK_EnableBefore).

Zorg ervoor dat de verbindingsreeks diagnostische gegevens aanwezig in de configuratie van de functie is. U kunt geen uitgebreide controle totdat u Diagnostisch hulpprogramma Azure inschakelt en een verbindingsreeks diagnostische hulpprogramma's in de configuratie van de functie opnemen inschakelen.   

> [AZURE.NOTE] Projecten hebt samengesteld Azure SDK 2,5 hebt de verbindingsreeks diagnostische gegevens niet automatisch opgenomen in de project-sjabloon. Voor deze projecten moet u handmatig de verbindingsreeks diagnostische gegevens toevoegen aan de configuratie van de functie.

**Diagnostisch hulpprogramma verbindingsreeks handmatig toevoegen aan configuratie van de functie**

1. Open het project-Cloudservice in Visual Studio
2. Dubbelklikt u op de **rol** naar de ontwerpfunctie voor rol openen en selecteer het tabblad **Instellingen**
3. Zoek naar een instelling met de naam **Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString**. 
4. Als deze instelling niet aanwezig is klikt u op de knop **Toevoegen instelling** toe te voegen aan de configuratie en het type voor de nieuwe instelling wijzigen naar **ConnectionString**
5. Stel de waarde voor de verbindingsreeks in het door te klikken op de knop **...** . Hiermee opent u een dialoogvenster waarop u kunt een opslag-account te selecteren.

    ![Instellingen voor Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioDiagnosticsConnectionString.png)

### <a name="to-change-the-monitoring-level-to-verbose-or-minimal"></a>Het niveau van de controle wijzigen naar uitgebreide of minimale

1. Open de pagina **configureren** voor de implementatie van de service cloud in de [klassieke Azure-portal](https://manage.windowsazure.com/).

2. Klik op **uitgebreid** of **minimale**in **niveau**. 

3. Klik op **Opslaan**.

Nadat u op het uitgebreide controle inschakelt, moet u beginnen met het zien van de controlegegevens in de portal van Azure klassieke binnen het uur.

De itemgegevens onbewerkte prestaties en geaggregeerde controlegegevens worden opgeslagen in de opslagruimte-account in tabellen die zijn gekwalificeerd door de implementatie-ID voor de rollen. 

## <a name="how-to-receive-alerts-for-cloud-service-metrics"></a>Hoe u: meldingen voor de doelstellingen van de cloud-service

U kunt op basis van uw cloudservice aan de doelstellingen voor controle waarschuwingen ontvangen. Klik op de pagina **Management-Services** van de Azure klassieke portal kunt u een regel om een waarschuwing wanneer de meetwaarde die u kiest een waarde die u opgeeft bereikt. U kunt er ook voor kiezen om e-mailbericht verzonden wanneer de melding wordt geactiveerd. Zie voor meer informatie [hoe: meldingen ontvangen en beheren waarschuwingsregels in Azure wordt aangegeven](http://go.microsoft.com/fwlink/?LinkId=309356).

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Hoe u: aan de doelstellingen toevoegen aan de tabel aan de doelstellingen

1. Open de **Monitor** -pagina voor de cloudservice in de [klassieke Azure-portal](http://manage.windowsazure.com/).

    De tabel aan de doelstellingen worden standaard een subset van de doelstellingen van de beschikbare weergegeven. De afbeelding ziet de uitgebreide doelstellingen van de standaard voor een cloudservice, beperkt tot het item van de prestaties geheugen beschikbare megabytes (MB) met gegevens op het niveau van de rol samengevoegd is. Gebruik **Toevoegen aan de doelstellingen** te extra statistische en rol niveau statistieken om te controleren in de portal van Azure klassieke selecteren.

    ![Uitgebreide weergeven](./media/cloud-services-how-to-monitor/CloudServices_DefaultVerboseDisplay.png)
 
2. Toevoegen aan de doelstellingen aan de tabel aan de doelstellingen:

    1. Klik op **Toevoegen aan de doelstellingen** voor het openen van **Kies aan de doelstellingen**, hieronder wordt weergegeven.

        De eerste beschikbare meetwaarde uitgevouwen zodat de opties die beschikbaar zijn. Voor elke metrisch, wordt in de bovenste optie geaggregeerde controlegegevens voor alle rollen bevat. Bovendien kunt u afzonderlijke rollen gegevens wilt weergeven.

        ![Toevoegen aan de doelstellingen](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics.png)

    2. Selecteren om weer te geven aan de doelstellingen

        - Klik op de pijl-omlaag door de meetwaarde om uit te vouwen de controleopties.
        - Schakel het selectievakje voor elke controle optie die u wilt weergeven.

        U kunt maximaal 50 aan de doelstellingen weergeven in de tabel aan de doelstellingen.

        > [AZURE.TIP] In het uitgebreide controle, bevatten de lijst aan de doelstellingen tientallen aan de doelstellingen. Als u wilt een schuifbalk weergeven, plaats u de muisaanwijzer boven de rechterkant van het dialoogvenster. Als u wilt filteren in de lijst, klikt u op het zoekpictogram en voer tekst in het zoekvak, zoals hieronder wordt weergegeven.
    
        ![Toevoegen aan de doelstellingen zoeken](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics_Search.png)


3. Nadat u klaar bent met het selecteren van de doelstellingen, klikt u op OK (vinkje).

    De geselecteerde parameters worden toegevoegd aan de tabel aan de doelstellingen, zoals hieronder wordt weergegeven.

    ![monitor aan de doelstellingen](./media/cloud-services-how-to-monitor/CloudServices_Monitor_UpdatedMetrics.png)

 
4. Als u wilt een meting verwijderen uit de tabel aan de doelstellingen, klikt u op de meetwaarde om deze te selecteren en klik vervolgens op **Verwijderen metrisch**. (Alleen ziet u **Metrisch verwijderen** als er een meting geselecteerd.)

### <a name="to-add-custom-metrics-to-the-metrics-table"></a>Aangepast aan de doelstellingen toevoegen aan de tabel aan de doelstellingen

De **uitgebreid** monitoring niveau bevat een lijst standaard parameters die u kunt controleren in de portal. Daarnaast kunt u een aangepast aan de doelstellingen of prestatie-items die zijn gedefinieerd door uw toepassing via de portal controleren.

De volgende stappen wordt ervan uitgegaan dat u **uitgebreid** niveau cmdlets voor controle hebt ingeschakeld en configureren van uw toepassing voor het verzamelen en overbrengen van aangepaste prestatie-items hebt. 

Als u wilt de aangepaste items in de portal weergeven die u wilt bijwerken van de configuratie in af-besturingselement-container:
 
1. Open de blob af-besturingselement-container in uw account van de opslagruimte diagnostische gegevens. U kunt Visual Studio of een andere opslag-explorer u dit wilt doen.

    ![Visual Studio Server Explorer](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioBlobExplorer.png)

2. Ga in het blob pad het patroon **DeploymentId/functienaam/RoleInstance** gebruiken om te vinden van de configuratie voor uw exemplaar van de rol. 

    ![Visual Studio opslag Explorer](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioStorage.png)
3. Download het configuratiebestand voor uw exemplaar van de rol en werk deze als u wilt opnemen van een aangepaste prestatie-items. Voorbeeld *voor schrijven Bytes/sec* controleren voor het *station C* toevoegen de volgende handelingen uit onder **PerformanceCounters\Subscriptions** knooppunt

    ```xml
    <PerformanceCounterConfiguration>
    <CounterSpecifier>\LogicalDisk(C:)\Disk Write Bytes/sec</CounterSpecifier>
    <SampleRateInSeconds>180</SampleRateInSeconds>
    </PerformanceCounterConfiguration>
    ```
4. De wijzigingen op te slaan en upload het configuratiebestand terug op dezelfde locatie het bestaande bestand in de blob wordt overschreven.
5. Schakelen naar de uitgebreide modus in de configuratie van Azure klassieke portal. Als u al in de uitgebreide modus waren moet u aan de minimale en terug naar de uitgebreide in-of uitschakelen.
6. De teller aangepaste prestaties wordt nu beschikbaar zijn in het dialoogvenster **Toevoegen aan de doelstellingen** . 

## <a name="how-to-customize-the-metrics-chart"></a>Hoe u: aanpassen van de grafiek aan de doelstellingen

1. Selecteer in de tabel aan de doelstellingen maximaal 6 maatstelsel uitzetten op de grafiek aan de doelstellingen. Als u wilt een meting hebt geselecteerd, klikt u op het selectievakje aan de linkerkant. Als u wilt een meting verwijderen uit de grafiek aan de doelstellingen, schakelt u het selectievakje in de tabel aan de doelstellingen.

    Terwijl u aan de doelstellingen in de tabel aan de doelstellingen selecteert, wordt het aan de doelstellingen worden toegevoegd aan de grafiek aan de doelstellingen. Een smalle weergegeven bevat een vervolgkeuzelijst **n meer** metrische kopteksten die niet, de weergave past.

 
2. Als u wilt schakelen tussen het weergeven van waarden (laatste waarde alleen voor elke metrisch) van de relatieve en absolute waarden (Y-as weergegeven), selecteer relatieve of Absolute boven aan de grafiek.

    ![Relatieve of Absolute](./media/cloud-services-how-to-monitor/CloudServices_Monitor_RelativeAbsolute.png)

3. Als u wilt wijzigen van het tijdsbereik Selecteer de aan de doelstellingen grafiek beeldschermen, 1 uur, 24 uur of 7 dagen boven aan de grafiek.

    ![Monitor met een weergave termijn](./media/cloud-services-how-to-monitor/CloudServices_Monitor_DisplayPeriod.png)

    Klik in het diagram aan de doelstellingen dashboard verschilt de methode voor het tekenen van de doelstellingen. Een standaardset met aan de doelstellingen beschikbaar is en aan de doelstellingen worden toegevoegd of verwijderd door de metrische koptekst selecteren.

### <a name="to-customize-the-metrics-chart-on-the-dashboard"></a>Om aan te passen van de grafiek aan de doelstellingen op het dashboard

1. Open het dashboard voor de cloudservice.

2. Toevoegen of verwijderen van de doelstellingen van de grafiek:

    - Als u wilt uitzetten in een nieuwe meting, selecteert u het selectievakje in voor de meetwaarde in de koptekst van de grafiek. Een smalle weergegeven, klikt u op de pijl-omlaag door ** *n*??metrics** wilt uitzetten van een meting die het koptekstgebied grafiek kan niet worden weergegeven.

    - Als u wilt verwijderen van een meting die zijn uitgezet op de grafiek, schakel het selectievakje op de koptekst.

3. Schakelen tussen **relatieve** en **Absolute** wordt weergegeven.

4. Kies 1 uur, 24 uur of zeven dagen gegevens wilt weergeven.

## <a name="how-to-access-verbose-monitoring-data-outside-the-azure-classic-portal"></a>Hoe u: Access uitgebreide buiten de Azure klassieke portal-gegevens bewaken

Uitgebreide controlegegevens wordt opgeslagen in tabellen in de opslagruimte-accounts die u voor elke rol opgeeft. Voor elke implementatie cloud-service worden voor de rol zes tabellen gemaakt. Twee tabellen worden gemaakt voor elke (5 minuten, 1 uur en 12 uur). Een van deze tabellen worden opgeslagen rol niveau aggregaties; Er worden aggregaties naar exemplaren van de rol opgeslagen in de andere tabel. 

De namen van de tabellen bestaan uit de volgende indeling:

```
WAD*deploymentID*PT*aggregation_interval*[R|RI]Table
```

waarbij geldt:

- *deploymentID* is de GUID die is toegewezen aan de implementatie van de cloud-service

- *aggregation_interval* = 5 M, 1U of 12 H

- rol niveau aggregaties = R

- aggregaties naar exemplaren van de rol RI =

De volgende tabellen zou bijvoorbeeld uitgebreide controlegegevens samengevoegd interval van 1 uur opslaan:

```
WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRTable (hourly aggregations for the role)

WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRITable (hourly aggregations for role instances)
```
