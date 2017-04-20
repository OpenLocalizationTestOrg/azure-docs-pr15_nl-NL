<properties
    pageTitle="Het controleren van een account opslag | Microsoft Azure"
    description="Leer hoe u een account opslagruimte in Azure controleren met behulp van de Azure-Portal."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="monitor-a-storage-account-in-the-azure-portal"></a>Monitor met een account opslag in de Portal van Azure

## <a name="overview"></a>Overzicht

U kunt uw opslag-account vanaf de [Portal van Azure](https://portal.azure.com)controleren. Als u uw account voor de opslag voor monitoring via de portal configureert, wordt in Azure opslag [Opslag Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx) gebruikt voor het bijhouden van de doelstellingen voor uw account en meld u aanvraaggegevens.

> [AZURE.NOTE]Extra kosten zijn gekoppeld aan de controlegegevens in de [Portal van Azure](https://portal.azure.com)onderzoeken. Zie <a href="http://msdn.microsoft.com/library/azure/hh360997.aspx">opslagruimte analyses en facturering</a>voor meer informatie. <br />

> Azure bestandsopslag momenteel ondersteunt opslag Analytics aan de doelstellingen, maar niet ondersteunt nog logboekregistratie. U kunt aan de doelstellingen voor de opslag van de Azure-bestanden via de [Portal van Azure](https://portal.azure.com)inschakelen.

> Opslag-accounts met een type herhaling van Zone-redundante opslag (ZRS) beschikt niet over de aan de doelstellingen of de mogelijkheid van de logboekregistratie is ingeschakeld op dit moment. 

> Zie voor een uitgebreide handleiding over het gebruik van opslagruimte analyses en andere hulpprogramma's voor identificeren en oplossen van problemen met Azure opslagruimte met een diagnose stellen bij [Monitor, automatisch opsporen, en problemen met Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).


## <a name="how-to-configure-monitoring-for-a-storage-account"></a>Hoe u: configureren voor een account opslagruimte bewaken

1. Klik in de [Portal van Azure](https://portal.azure.com) **opslagruimte**op en klik vervolgens op de naam van het opslag-account te openen van het dashboard.

2. Klik op **configureren**en schuif omlaag naar de **controle** -instellingen voor de blob, de tabel en de wachtrijservices.

    ![MonitoringOptions](./media/storage-monitor-storage-account/Storage_MonitoringOptions.png)

3. Stel het niveau van de cmdlets voor controle en het bewaarbeleid voor gegevens voor elke service in **monitoring**:

    -  Als u het niveau van de controle, selecteert u een van de volgende opties:

      **Minimale** - verzameld aan de doelstellingen zoals ingress/egress, beschikbaarheid latentie en success percentages, die voor de blob, de tabel en de wachtrijservices worden samengevoegd.

      **Uitgebreid** - verzamelt naast de doelstellingen van de minimale dezelfde reeks aan de doelstellingen voor elke opslagbewerking in de Service-API van Azure opslag. Uitgebreide aan de doelstellingen inschakelen nabij analyse van problemen die tijdens de bewerkingen van toepassingen optreden.

      **Uit** - uitgeschakeld bewaken. Bestaande gegevens bewaken blijft behouden tot het einde van de bewaarperiode.

- Als u wilt het bewaarbeleid voor gegevens instellen in **bewaarbeleid (in dagen)**, typt u het aantal dagen van de gegevens moeten worden bewaard van 1 tot 365 dagen. Als u niet instellen van een bewaarbeleid wilt, voert u nul. Als er geen bewaarbeleid, is deze aan u de controlegegevens verwijderen. Het is raadzaam een bewaarbeleid op basis van hoe lang u bewaren van opslag analytics-gegevens voor uw account wilt, zodat de oude en niet-gebruikte analytics-gegevens kunnen worden verwijderd door systeem gratis instellen.

4. Wanneer u klaar bent met de configuratie controleren, klikt u op **Opslaan**.

U moet beginnen ziet op het dashboard en de pagina **Monitor** na ongeveer een uur-gegevens bewaken.

Totdat u voor een account opslag voor controle configureert, geen controlegegevens worden verzameld en de grafieken de doelstellingen op de pagina van de **Monitor** en dashboard leeg zijn.

Nadat u de controle niveaus en het bewaarbeleid hebt ingesteld, kunt u kiezen welke van de beschikbare maatstelsel controleren in de [Portal van Azure](https://portal.azure.com)en welke maatstelsel uitzetten op de doelstellingen grafieken. Een standaardset met aan de doelstellingen wordt weergegeven op elk niveau van de controle. U kunt **Toevoegen aan de doelstellingen** toevoegen of verwijderen van de doelstellingen uit de lijst aan de doelstellingen.

Aan de doelstellingen zijn opgeslagen in de opslagruimte-account in vier tabellen met de naam $MetricsTransactionsBlob, $MetricsTransactionsTable $MetricsTransactionsQueue en $MetricsCapacityBlob. Zie voor meer informatie [Over Analytics opslageenheden](http://msdn.microsoft.com/library/azure/hh343258.aspx).


## <a name="how-to-customize-the-dashboard-for-monitoring"></a>Hoe: het dashboard voor het controleren van aanpassen

U kunt maximaal zes maatstelsel uitzetten op de grafiek aan de doelstellingen van negen beschikbare maatstelsel kiezen op het dashboard. Voor elke service (blob, tabel en wachtrij) zijn de doelstellingen van de beschikbaarheid, Success Percentage en totaal aantal aanvragen beschikbaar. De beschikbaar op het dashboard van de doelstellingen zijn hetzelfde voor de minimale of uitgebreide controle.

1. Klik in de [Portal van Azure](https://portal.azure.com) **opslagruimte**op en klik vervolgens op de naam van de opslag-account te openen van het dashboard.

2. Als u wilt wijzigen van de criteria die zijn uitgezet op de grafiek, voert u een van de volgende bewerkingen uit:

    - Als u wilt een nieuwe meting toevoegen aan de grafiek, klikt u op het gekleurde selectievakje in naast de metrische koptekst in de tabel onder de grafiek.

    - Als u wilt verbergen een meting die zijn uitgezet op de grafiek, schakel het selectievakje gekleurde naast de metrische koptekst.

        ![Monitoring_nmore](./media/storage-monitor-storage-account/storage_Monitoring_nmore.png)

3. De grafiek worden standaard weergegeven trends, weergeven van alleen de huidige waarde van elke meting (de **relatieve** optie boven aan de grafiek). Als u wilt een Y-as hebt weergegeven zodat u absolute waarden kunt zien, selecteert u **Absolute**.

4. Als u wilt wijzigen van het tijdsbereik Selecteer de aan de doelstellingen grafiek beeldschermen, 6 uur, 24 uur of 7 dagen boven aan de grafiek.


## <a name="how-to-customize-the-monitor-page"></a>Hoe u: de Monitor pagina aanpassen

U kunt de volledige set aan de doelstellingen voor uw account opslagruimte weergeven op de pagina **Monitor** .

- Als uw account opslagruimte minimale monitoring is geconfigureerd heeft, worden de doelstellingen zoals ingress/egress, beschikbaarheid, latentie en success percentages van de blob, de tabel en de wachtrijservices samengevoegd.

- Als uw account opslagruimte uitgebreide controle is geconfigureerd heeft, is het aan de doelstellingen zijn beschikbaar met een betere resolutie van afzonderlijke opslagbewerkingen naast de service level aggregaties.

Gebruik de volgende procedures uit om te kiezen welke opslageenheden weergeven in de aan de doelstellingen grafieken en tabellen die worden weergegeven op de pagina **Monitor** . Deze instellingen zijn niet van invloed op de siteverzameling, aggregatie en opslag van gegevens in het account opslagruimte bewaken.

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Hoe u: aan de doelstellingen toevoegen aan de tabel aan de doelstellingen


1. Klik in de [Portal van Azure](https://portal.azure.com) **opslagruimte**op en klik vervolgens op de naam van de opslag-account te openen van het dashboard.

2. Klik op **controleren**.

    De **Monitor** -pagina wordt geopend. Standaard weergegeven in de tabel aan de doelstellingen een subset van de criteria die beschikbaar voor controle zijn. De afbeelding ziet de standaardweergave van de Monitor voor een account opslagruimte met uitgebreide controle is geconfigureerd voor alle drie services. Gebruik **Toevoegen aan de doelstellingen** te selecteren van de criteria die u wilt controleren van alle beschikbare maatstelsel.

    ![Monitoring_VerboseDisplay](./media/storage-monitor-storage-account/Storage_Monitoring_VerboseDisplay.png)

    > [AZURE.NOTE] Houd rekening met kosten wanneer u het aan de doelstellingen selecteert. Er zijn transactie en egress kosten die is gekoppeld aan het vernieuwen controleren wordt weergegeven. Zie [opslagruimte analyses en facturering](http://msdn.microsoft.com/library/azure/hh360997.aspx)voor meer informatie.

3. Klik op **toevoegen aan de doelstellingen**.

    De statistische parameters die beschikbaar in de minimale monitoring zijn zijn boven aan de lijst. Als het selectievakje is geselecteerd, wordt de meetwaarde wordt weergegeven in de lijst aan de doelstellingen.

    ![AddMetricsInitialDisplay](./media/storage-monitor-storage-account/Storage_AddMetrics_InitialDisplay.png)

4. Plaats de muisaanwijzer op de rechterkant van het dialoogvenster om weer te geven van een schuifbalk die u als u wilt schuiven extra statistieken in beeld kunt slepen.

    ![AddMetricsScrollbar](./media/storage-monitor-storage-account/Storage_AddMetrics_Scrollbar.png)


5. Klik op de pijl-omlaag door een meting om uit te vouwen van een lijst met bewerkingen die de meetwaarde is beperkt als u wilt opnemen. Selecteer elke bewerking die u wilt weergeven in de tabel aan de doelstellingen van de [Azure-Portal](https://portal.azure.com).

    In de volgende afbeelding, is de meetwaarde AUTORISATIE fout uitgevouwen.

    ![ExpandCollapse](./media/storage-monitor-storage-account/Storage_AddMetrics_ExpandCollapse.png)


6. Nadat u aan de doelstellingen voor alle services selecteert, klikt u op OK (vinkje) als u wilt bijwerken van de controle configuratie. De geselecteerde parameters worden toegevoegd aan de tabel aan de doelstellingen.

7. Als een meting uit de tabel wilt verwijderen, klikt u op de meetwaarde om deze te selecteren en klik vervolgens op **Metrisch verwijderen**.

    ![DeleteMetric](./media/storage-monitor-storage-account/Storage_DeleteMetric.png)

## <a name="how-to-customize-the-metrics-chart-on-the-monitor-page"></a>Hoe u: aanpassen van de grafiek aan de doelstellingen op de pagina controleren

1. Selecteer op de pagina **Monitor** voor de opslag-account, in de tabel aan de doelstellingen maximaal 6 maatstelsel uitzetten op de grafiek aan de doelstellingen. Als u wilt een meting hebt geselecteerd, klikt u op het selectievakje aan de linkerkant. Als u wilt een meting verwijderen uit de grafiek, schakel het selectievakje.

2. Als u wilt schakelen van de grafiek tussen relatieve waarden (laatste waarde wordt alleen weergegeven) en absolute waarden (Y-as weergegeven), selecteert u de **relatieve** of **Absolute** boven aan de grafiek.

3.  Als u wilt wijzigen de tijd variëren de aan de doelstellingen grafiek bevat, selecteer **6 uur**, **24 uur**of **7 dagen** boven aan de grafiek.



## <a name="how-to-configure-logging"></a>Hoe u: gegevens vastleggen configureren

Voor elk van de services opslagruimte beschikbaar voor communicatie met uw account opslag (blob, tabel en wachtrij), u kunt naar diagnostische logboeken opslaan voor aanvragen lezen, schrijven aanvragen en/of Delete-aanvragen en het bewaarbeleid voor gegevens kunt instellen voor elk van de services.

1. Klik in de [Portal van Azure](https://portal.azure.com) **opslagruimte**op en klik vervolgens op de naam van de opslag-account te openen van het dashboard.

2. Klik op **configureren**en gebruik de pijl-omlaag op het toetsenbord om het Schuif omlaag naar **logboekregistratie**.

    ![Storagelogging](./media/storage-monitor-storage-account/Storage_LoggingOptions.png)


3. Voor elke service (blob, tabel en wachtrij), door het volgende te configureren:

    - De soorten verzoek om aan te melden: aanvragen lezen, schrijven verzoeken en aanvragen verwijderen.

    - Het aantal dagen worden bewaard van gegevens in het logboek. Voer nul is als u niet wilt voor het instellen van een bewaarbeleid. Als u een bewaarbeleid niet instelt, is deze aan u de logboeken verwijderen.

4. Klik op **Opslaan**.

De logboeken aan de diagnostische gegevens worden opgeslagen in een blob container met de naam $logs in uw account opslag. Zie [Opslagruimte Analytics logboekregistratie](http://msdn.microsoft.com/library/azure/hh343262.aspx)voor informatie over toegang tot de container $logs.
