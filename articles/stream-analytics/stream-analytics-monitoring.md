<properties 
    pageTitle="Lidmaatschap Stream Analytics taak controleren | Microsoft Azure" 
    description="Lidmaatschap Stream Analytics taak controleren" 
    keywords="monitor met een query"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Weten Stream Analytics taak bewaken en hoe u query's controleren

## <a name="introduction-the-monitor-page"></a>Inleiding: De monitor-pagina

De beheerportal van Azure en Azure-Portal oppervlak voornaamste prestatiestatistieken ophalen die kunnen worden gebruikt om te controleren en problemen met de prestaties van uw query en taak. 

Klik op het tabblad **Beeldscherm** van een lopende Stream Analytics-taak om deze gegevens weer te geven in de portal Azure Management. Er treedt een vertraging van het meeste 1 minuut in de prestatiegegevens niet weergegeven in de Monitor-pagina.  

  ![Taak Dashboard bewaken](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

Blader naar de Stream Analytics-taak u geïnteresseerd bent in zien aan de doelstellingen voor en de sectie **controle** weergeven in de Portal Azure.  

  ![Azure-Portal Monitoring taak Dashboard](./media/stream-analytics-monitoring/06-stream-analytics-monitoring.png)  

De eerste keer die een Stream Analytics-taak is gemaakt in een gebied, moet u voor het configureren van diagnostische hulpprogramma's voor die regio. Klik hiertoe wordt op een willekeurige plaats in de sectie **controle** en het blad **Diagnostische gegevens** weergegeven. Hier kunt u diagnostische gegevens inschakelen en geef een account opslag voor gegevens bewaken.  

  ![Azure-Portal configureren query diagnostische gegevens](./media/stream-analytics-monitoring/07-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Aan de doelstellingen van analysegegevens Stream beschikbaar


| Metrisch | Definitie |
|--------|-------------|
| SO % gebruik | Het gebruik van de eenheid Streaming toegewezen aan een taak op het tabblad schaal van de taak. Deze indicator bereikt moet 80% of bovenstaande hoge kans die gebeurtenis verwerking kan worden vertraagd of gestopt bezig is. |
| Invoer gebeurtenissen | Hoeveelheid gegevens die zijn ontvangen door de Stream Analytics-taak, bij number van gebeurtenissen. Dit kan worden gebruikt voor het valideren van dat gebeurtenissen worden verzonden naar de invoerbron. |
| Uitvoer gebeurtenissen | Hoeveelheid gegevens die zijn verzonden door de Stream Analytics-taak naar de doelsite uitvoer, bij number van gebeurtenissen. |
| Out volgorde gebeurtenissen | Aantal gebeurtenissen ontvangen uit volgorde die zijn verwijderd of een gecorrigeerde tijdstempel, op basis van het beleid voor het bestellen van gebeurtenis gegeven. Dit kan worden beïnvloed door de configuratie van de instelling bij aantal volgorde tolerantie venster. |
| Gegevens conversiefouten | Het aantal gegevens conversiefouten weergegeven die zijn gemaakt door een Stream Analytics-taak. |
| Runtime-fouten | Het aantal fouten die tijdens de uitvoering van een taak Stream Analytics optreden. |
| Vertraagde invoer gebeurtenissen | Aantal gebeurtenissen binnengekomen latere uit de bron die al is verbroken of hun tijdstempel is gewijzigd op basis van de configuratie van de gebeurtenis ordening beleid van de instelling latere ontvangst van tolerantie venster. |

## <a name="customizing-monitoring-in-the-azure-management-portal"></a>Controle aanpassen in de beheerportal van Azure ##

Snel aan de doelstellingen van 6 kan worden weergegeven in een grafiek.

Als u wilt schakelen tussen het weergeven van waarden (laatste waarde alleen voor elke metrisch) van de relatieve en absolute waarden (Y-as weergegeven), selecteer relatieve of Absolute boven aan de grafiek.

  ![Relatieve Absolute monitor met een query](./media/stream-analytics-monitoring/02-stream-analytics-monitoring.png)  

Aan de doelstellingen kunnen worden weergegeven in de grafiek Monitor in aggregaties 1 uur, 12 uur, 24 uur of 7 dagen.

Als u wilt wijzigen van het tijdsbereik Selecteer de aan de doelstellingen grafiek beeldschermen, 1 uur, 24 uur of 7 dagen boven aan de grafiek.

  ![Monitor met een query tijdschaal](./media/stream-analytics-monitoring/03-stream-analytics-monitoring.png)  

U kunt regels die u kunnen een melding per e-mail geval de taak een opgegeven drempelwaarde kruist instellen. 

## <a name="customizing-monitoring-in-the-azure-portal"></a>Cmdlets voor controle in de Portal van Azure aanpassen ##

U kunt aanpassen van het type grafiek, wordt weergegeven, aan de doelstellingen en tijdbereik in de grafiek bewerken-instellingen. Lees [hoe u het Monitoring aanpassen](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md)voor meer informatie.

  ![Azure Portal Query Monitor tijdschaal](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  

## <a name="job-status"></a>Taakstatus

De status van Stream Analytics taken kan worden weergegeven in de klassieke Azure-Portal waar u een van taken overzicht. U kunt de lijst met taken ziet door te klikken op de Stream Analytics-pictogram in de klassieke Azure-Portal.

| Status | Definitie |
|--------|------------|
| Gemaakt | Een taak is gemaakt, maar niet is gestart. |
| Starten | Een gebruiker hebt geklikt op Start het project en de taak wordt gestart |
| Uitgevoerd | De taak is toegewezen, invoer verwerkt of wachten op de verwerking van invoer. Als de taak ziet u een staat uitgevoerd zonder dat er uitvoer, is deze waarschijnlijk dat het tijdvenster gegevensverwerking groot is of de query-logica ingewikkeld. Een andere reden dan ook mogelijk dat momenteel is er geen gegevens naar de taak is verzonden. |
| Stoppen | Een gebruiker hebt geklikt op Stop de taak en de taak wordt gestopt. |
| Gestopt | De taak is gestopt. |
| Niet beschikbaar is weergegeven | Deze status geeft aan dat een taak Stream Analytics tijdelijke is fouten (voor ex. Invoer/uitvoer fouten, verwerkingsfouten, conversiefouten enzovoort). De taak nog steeds wordt uitgevoerd, maar er zijn echter een groot aantal fouten wordt gegenereerd. Aandacht klant voor deze taak en de klant de logboeken aan de bewerkingen van de fouten kan zien. |
| Is mislukt | Hiermee wordt aangeduid dat de taak is mislukt vanwege fouten en de verwerking is gestopt. De klant moet zoeken in de logboeken bewerkingen om de fouten opsporen. |
| Verwijderen | Hiermee wordt aangeduid dat de taak wordt verwijderd. |

## <a name="diagnosis"></a>Diagnose

Klik in de portal Azure Management vindt het dashboard taak u informatie over waar u nodig hebt om te zoeken voor de diagnose, dat wil zeggen invoeritems output en/of meld u aan de bewerkingen. U kunt klikken op de koppeling naar de juiste locatie de diagnose nagaan gaan.

  ![Monitor met een query fout](./media/stream-analytics-monitoring/04-stream-analytics-monitoring.png)  

Klikken op de resource invoer- en uitvoerbereik bevat gedetailleerde diagnostische gegevens. Dit is vernieuwd met de meest recente informatie voor de diagnose terwijl de taak wordt uitgevoerd.

  ![Query diagnostische gegevens](./media/stream-analytics-monitoring/05-stream-analytics-monitoring.png)  

## <a name="get-help"></a>U kunt hulp krijgen
Voor hulp, probeert u onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)
