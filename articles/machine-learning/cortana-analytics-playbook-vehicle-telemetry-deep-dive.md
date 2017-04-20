<properties 
    pageTitle="Voertuig telemetrielogboek analytics oplossing playbook: uitgebreide Kennismaking in de oplossing | Microsoft Azure" 
    description="De mogelijkheden van Cortana Intelligence gebruiken om te krijgen van realtime en bekijk inzichten in een voertuig gezondheid en sturende voorkeur werkwijze." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a>Voertuig telemetrielogboek analytics oplossing playbook: uitgebreide Kennismaking in de oplossing

In dit **menu** -koppelingen naar de secties van dit playbook: 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

In dit gedeelte boren ingedrukt in elk van de fasen weergegeven in de oplossingsarchitectuur met instructies en tips voor het aanpassen. 

## <a name="data-sources"></a>Gegevensbronnen

De oplossing gebruikt twee verschillende gegevensbronnen:

- **gesimuleerd voertuig signalen en diagnostische gegevensset** en 
- **catalogus met een voertuig**

Een voertuig telematica simulator maakt deel uit van deze oplossing. Er genereert diagnostische informatie en geeft de status van het voertuig als het een patroon op een bepaald moment in tijd overeenkomt. Klik op [Een voertuig telematica Simulator](http://go.microsoft.com/fwlink/?LinkId=717075) als u wilt downloaden van de **Voertuig telematica Simulator Visual Studio-oplossing** voor aanpassingen op basis van uw vereisten. De catalogus voertuig bevat een gegevensset verwijzing met een Chassisnummer model toewijzen aan.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig2-vehicle-telematics-simulator.png)

*Afbeelding 2 – voertuig telematica Simulator*

Dit is een gegevensset JSON-indeling, die het volgende schema bevat.

Kolom | Beschrijving | Waarden 
 ------- | ----------- | --------- 
CHASSISNUMMER | Het identificatienummer willekeurig wordt gegenereerd voertuig | Hiermee wordt opgehaald uit een hoofdlijst met 10.000 willekeurig wordt gegenereerd voertuig id-nummers.
Buiten temperatuur | De externe temperatuur waar het voertuig is sturende | Willekeurig wordt gegenereerd getal tussen 0-100
Engine temperatuur | De temperatuur engine van het voertuig | Willekeurig wordt gegenereerd getal tussen 0-500
Snelheid | De snelheid van de engine waarop het voertuig is sturende | Willekeurig wordt gegenereerd getal tussen 0-100
Brandstof | De brandstofpeil van het voertuig | Willekeurig wordt gegenereerd getal tussen 0-100 (geeft brandstof niveau percentage)
EngineOil | Het niveau van de olive oil engine van het voertuig | Willekeurig wordt gegenereerd getal tussen 0-100 (geeft engine olive oil niveau percentage)
Druk band | Druk op de band van het voertuig | Gegenereerde willekeurig getal van 0 tot 50 (geeft band druk niveau percentage)
Kilometerstand | De kilometerstand van het voertuig | Willekeurig wordt gegenereerd getal tussen 0-200000
Accelerator_pedal_position | De accelerator vorm positie van het voertuig | Willekeurig wordt gegenereerd getal tussen 0-100 (geeft accelerator niveau percentage)
Parking_brake_status | Geeft aan of het voertuig al dan niet is geparkeerd | Waar of ONWAAR
Headlamp_status | Geeft aan waar het licht is ingeschakeld of niet | Waar of ONWAAR
Brake_pedal_status | Geeft aan of het rempedaal al dan niet wordt ingedrukt | Waar of ONWAAR
Transmission_gear_position | De positie van het tandwiel overdracht van het voertuig | Staten: eerste, tweede, derde, vierde, vijfde, zesde, zevende, achtste
Ignition_status | Hiermee wordt aangegeven of het voertuig uitgevoerd of is gestopt | Waar of ONWAAR
Windshield_wiper_status | Geeft aan of de combinatie voorruit al dan niet is ingeschakeld | Waar of ONWAAR
ABS | Geeft aan of ABS al dan niet is ingeschakeld | Waar of ONWAAR
Tijdstempel | Het tijdstip waarop het gegevenspunt wordt gemaakt | Datum
Plaats | De locatie van het voertuig | 4 steden in deze oplossing: Bellevue, Redmond, Sammamish, Seattle


VIN bevat de gegevensset voertuig model verwijzing naar de modeltoewijzing. 

CHASSISNUMMER | Model |
--------------|------------------
FHL3O1SA4IEHB4WU1 | Sedan |
8J0U8XCPRGW4Z3NQE | Hybride |
WORG68Z2PLTNZDBI7 | Family sedan |
JTHMYHQTEPP4WBMRN | Sedan |
W9FTHG27LZN1YWO0Y | Hybride |
MHTP9N792PHK08WJM | Family sedan |
EI4QXI2AXVQQING4I | Sedan |
5KKR2VB4WHQH97PF8 | Hybride |
W9NSZ423XZHAONYXB | Family sedan |
26WJSGHX4MA5ROHNL | Geconverteerd |
GHLUB6ONKMOSI7E77 | Station Wagon |
9C2RHVRVLMEJDBXLP | Compacte auto |
BRNHVMZOUJ6EOCP32 | Kleine standaard |
VCYVW0WUZNBTM594J | Sportauto |
HNVCE6YFZSA5M82NY | Middellange standaard |
4R30FOR7NUOBL05GJ | Station Wagon |
WYNIIY42VKV6OQS1J | Grote standaard |
8Y5QKG27QET1RBK7I | Grote standaard |
DF6OX2WSRA6511BVG | Coupé |
Z2EOZWZBXAEW3E60T | Sedan |
M4TV6IEALD5QDS3IR | Hybride |
VHRA1Y2TGTA84F00H | Family sedan |
R0JAUHT1L1R3BIKI0 | Sedan |
9230C202Z60XX84AU | Hybride |
T8DNDN5UDCWL7M72H | Family sedan |
4WPYRUZII5YV7YA42 | Sedan |
D1ZVY26UV2BFGHZNO | Hybride |
XUF99EW9OIQOMV7Q7 | Family sedan
8OMCL3LGI7XNCC21U | Geconverteerd |
…….  |   |


### <a name="to-generate-simulated-data"></a>Om gesimuleerd gegevens te genereren
1.  Als u wilt de gegevens simulator-pakket downloaden, klikt u op de pijl in de rechterbovenhoek van het knooppunt voertuig telematica Simulator. Opslaan en pak de bestanden lokaal op uw computer. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig3-vehicle-telemetry-blueprint.png)*Afbeelding 3 – voertuig Telemetrielogboek Analytics oplossing blauwdruk*

2.  Op de lokale computer, gaat u naar de map waarin u het voertuig telematica Simulator-pakket hebt uitgepakt. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-simulator-folder.png)*Afbeelding 4 – map voertuig telematica Simulator*

3.  De toepassing **CarEventGenerator.exe**uitvoeren.

### <a name="references"></a>Verwijzingen

[Voertuig telematica Simulator Visual Studio-oplossing](http://go.microsoft.com/fwlink/?LinkId=717075) 

[Azure gebeurtenis Hub](https://azure.microsoft.com/services/event-hubs/)

[Azure gegevens Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/)


## <a name="ingestion"></a>Opname
Combinaties van Azure gebeurtenis Hubs, Stream Analytics en gegevens Factory wordt gebruikt om de signalen voertuig, de diagnostische gebeurtenissen, nemen en realtime en analyses batch. Alle onderdelen zijn gemaakt en geconfigureerd als onderdeel van de oplossing-implementatie. 

### <a name="real-time-analysis"></a>Realtime analyse
De gebeurtenissen die zijn gegenereerd door de voertuig telematica Simulator zijn gepubliceerd naar de gebeurtenis Hub met behulp van de gebeurtenis Hub SDK. De Stream Analytics-taak deze gebeurtenissen van de Hub gebeurtenis ingests en de gegevens in realtime om de status van een voertuig te analyseren worden verwerkt. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-event-hub-dashboard.png) 

*Afbeelding 5 - gebeurtenis Hub dashboard*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-processing-data.png) 

*Afbeelding 6 - Stream analytics taak gegevensverwerking*

De stream analytics-taak.

- gegevens uit de Hub gebeurtenis ingests 
- een join met de verwijzingsgegevens naar het voertuig Chassisnummer toewijzen aan het bijbehorende model uitvoert 
- zich blijft voordoen ze in Azure-blobopslag voor uitgebreide batch analytics. 

De volgende stream analytics-query wordt gebruikt om de gegevens in Azure-blobopslag. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

*Afbeelding 7 - Stream analytics taak query voor de opname van gegevens*

### <a name="batch-analysis"></a>Batchanalyse
We zijn ook een extra hoeveelheid gesimuleerd voertuig signalen en diagnostische dataset van uitgebreidere batch analysegegevens genereren. Dit is vereist om ervoor te zorgen het gegevensvolume van een goede vertegenwoordiger voor batchbestand. Hiertoe gebruiken we een pijplijn met de naam 'PrepareSampleDataPipeline' in de werkstroom Azure gegevens Factory te genereren van één jaar lang gesimuleerd voertuig signalen en diagnostische gegevensset. Klik op [gegevens Factory aangepaste activiteit](http://go.microsoft.com/fwlink/?LinkId=717077) te downloaden van de gegevens Factory aangepaste DotNet activiteit Visual Studio-oplossing voor aanpassingen op basis van uw vereisten. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

*Figuur 8 - voorbeeldgegevens voorbereiden voor batch verwerking werkstroom*

De pijplijn bestaat uit een aangepaste ADF .net activiteit, hier weergeven:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline.png) 

*Afbeelding 9 - PrepareSampleDataPipeline*

Zodra de pijplijn met succes uitgevoerd en "RawCarEventsTable" gegevensset is gemarkeerd als 'Gereed', zegt jaarabonnement van gesimuleerd voertuig signalen en diagnostische worden gegevens geproduceerd. Ziet u de volgende map en het bestand dat is gemaakt in uw account opslag onder de container "connectedcar":

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

*Afbeelding 10 - PrepareSampleDataPipeline uitvoer*

### <a name="references"></a>Verwijzingen

[Azure gebeurtenis Hub SDK voor stream opname](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

[Azure mogelijkheden om gegevens Factory gegevens verkeer](../data-factory/data-factory-data-movement-activities.md)
[Azure gegevens Factory DotNet activiteit](../data-factory/data-factory-use-custom-activities.md)

[Azure gegevens Factory DotNet activiteit visual studio-oplossing voor het voorbereiden van voorbeeldgegevens](http://go.microsoft.com/fwlink/?LinkId=717077) 


## <a name="partition-the-dataset"></a>Partition de gegevensset

De onbewerkte gedeeltelijk gestructureerd voertuig signalen en diagnostische gegevensset zijn partitioneren in de stap voor het voorbereiden van gegevens in een jaar/maand-indeling. Deze partitioneren verhoogt het niveau efficiënter query's uitvoeren en scalable langdurige opslagruimte doordat foutenstructuuranalyse-over van één blob account naar de volgende terwijl het eerste account wordt opgevuld. 

>[AZURE.NOTE] Deze stap in de oplossing is alleen van toepassing op batchbestand.

Invoer en uitvoer gegevens beheren van gegevens:

- De **uitvoergegevens** (aangeduid *PartitionedCarEventsTable*) is een lange periode worden bewaard als het formulier moeten beschikken over / "rawest" van gegevens in het van klant "gegevens meer'. 
- De **invoergegevens** naar deze verkooppijplijn zou meestal worden verwijderd, omdat de uitvoergegevens volledige leiden naar de invoer heeft-alleen deze opgeslagen wordt (partitioneren) beter voor later gebruik.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-workflow.png)

*Afbeelding 11 – Partition auto gebeurtenissen werkstroom*

De onbewerkte gegevens is het gebruik van de activiteit in een component HDInsight in "PartitionCarEventsPipeline" partitioneren. De voorbeeldgegevens gegenereerd in stap 1 voor een jaar is partitioneren per jaar/maand. De partities worden gebruikt om te genereren voertuig signalen en diagnostische gegevens voor elke maand (totale 12 partities) van een jaar. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partition-car-events-pipeline.png)

*Figuur 12 - PartitionCarEventsPipeline*

De volgende component-script, met de naam 'partitioncarevents.hql', wordt gebruikt voor het partitioneren en bevindt zich in de map '\demo\src\connectedcar\scripts' van de gedownloade zip. 


    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string,
                YearNo                          int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

*Afbeelding 13 - component PartitionConnectedCarEvents Script*

Zodra de pijplijn is uitgevoerd, kunt u de volgende partities gegenereerd in uw account opslag onder de container "connectedcar" zien.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-partitioned-output.png)

*Figuur 14 - gepartitioneerde uitvoer*

De gegevens nu is geoptimaliseerd, meer beheerbaar is en klaar voor nadere verwerking om te uitgebreide batch inzicht krijgen. 

## <a name="data-analysis"></a>Gegevensanalyse

In dit gedeelte ziet u hoe u het combineren van Azure Stream analyses, Azure Machine Learning Factory van Azure-gegevens en Azure HDInsight voor uitgebreide geavanceerde analyses over voertuig status en sturende voorkeur werkwijze. Er zijn drie subsecties hier:

1.  **Machine Learning**: deze subsectie bevat informatie over het afwijking detectie experiment die wordt gebruikt in deze oplossing te voorspellen voertuigen vereisen onderhoud onderhoud en voertuigen terughalen door problemen met de veiligheid vereisen.
2.  **Realtime analyse**: deze subsectie bevat informatie over het gebruik van de Stream Analytics-querytaal en het machine learning-experiment met een aangepaste toepassing realtime dwarsas de realtime analytische gegevens.
3.  **Batchanalyse**: deze subsectie bevat informatie met betrekking tot de transformeren en verwerking van de batch-gegevens met behulp van Azure HDInsight en Azure Machine Learning geoperationaliseerd door Factory van Azure-gegevens.

### <a name="machine-learning"></a>Machine Learning

Hier ons doel is om te voorspellen de voertuigen waarvoor onderhoud of intrekken op basis van bepaalde Health statistische gegevens. We geven de volgende veronderstellingen

- Als een van de volgende drie voorwaarden waar zijn, wordt de voertuigen vereisen **onderhoud onderhoud**:
    - Druk band is laag
    - Engine olive oil niveau is laag
    - Engine temperatuur is hoog

- Als een van de volgende voorwaarden voldaan wordt, kunnen de voertuigen heb een **probleem veiligheid** en vereisen **intrekken**:
    - Engine temperatuur hoog is, maar buiten temperatuur is laag
    - Engine temperatuur is laag maar buiten temperatuur hoog is

Op basis van de vorige vereisten, hebben we twee afzonderlijke modellen om op te sporen afwijkingen, een voor een voertuig onderhoud detectie en een voor een voertuig intrekken detectie gemaakt. In beide deze modellen, wordt de algoritme van de ingebouwde Principal onderdeel analyse Programmacompatibiliteit gebruikt voor afwijking detectie. 

**Onderhoud detectie model**

Als een van de drie indicatoren - band druk, engine olive oil of engine temperatuur - voldoet aan de desbetreffende voorwaarde, wordt in het model van de detectie onderhoud een afwijking rapporten. Hierdoor kunnen alleen moeten we rekening houden met deze drie variabelen in het samenstellen van het model. In ons experiment in Azure Machine Learning gebruiken we eerst een module **Kolommen selecteren in de gegevensset** worden geëxtraheerd deze drie variabelen. Vervolgens gebruiken we de afwijking PCA gebaseerde detectiemodule maken van het model van de detectie afwijking. 

Hoofdsom onderdeel analyse (VO) is een techniek waarop deze is opgezet in machine learning die kan worden toegepast op de Functieselectie, indeling en afwijking detectie. PCA converteert een reeks hoofdletters/kleine letters met mogelijk gecorreleerde variabelen in een verzameling waarden principal onderdelen genoemd. De belangrijkste idee van modellering PCA gebaseerde is tot projectgegevens naar een lager dimensionaal ruimte zodat functies en afwijkingen kunnen gemakkelijker worden geïdentificeerd.
 
Voor elke nieuwe invoer aan het model detectie, de afwijking-detectie berekent eerst de raming op de eigenvectors en vervolgens berekent de genormaliseerde herstel-fout. Deze fout genormaliseerde is afwijking score. Hoe hoger de fout, de afwijkende wordt het exemplaar is. 

In het onderhoud detectie probleem, kan elke record worden beschouwd als een punt in een 3-dimensionaal ruimte gedefinieerd door band druk, engine olive oil en engine temperatuur coördinaten. Deze afwijkingen vastleggen, kunnen we de oorspronkelijke gegevens in de 3-dimensionaal ruimte project naar een 2D-ruimte PCA gebruiken. Dus instellen we de parameter aantal onderdelen in PCA 2 gebruiken. Deze parameter speelt een belangrijke rol in afwijking PCA gebaseerde detectie toepassen. Na het projecteren gegevens met behulp van PCA, kunnen we deze afwijkingen eenvoudiger vaststellen.

**Afwijking detectie model intrekken** In het model intrekken afwijking detectie, gebruiken we de kolommen selecteren in de gegevensset en afwijking PCA gebaseerde detectiemodules op een soortgelijke manier. We eerst uitpakken drie variabelen - engine temperatuur, buiten temperatuur en snelheid - gebruik van de module **Kolommen selecteren in de gegevensset** name. We ook de variabele snelheid sinds de temperatuur engine meestal gerelateerd is aan de snelheid. Vervolgens gebruiken we afwijking PCA gebaseerde detectiemodule wilt de gegevens uit de 3-dimensionaal ruimte projecteren op een 2D spatie. Het intrekken criterium is voldaan en dus intrekken door het voertuig is vereist wanneer engine temperatuur en externe temperatuur elkaar zijn zeer negatief gekoppeld. Afwijking PCA gebaseerde detectiealgoritme gebruikt, kunnen we de afwijkingen na het uitvoeren van PCA vastleggen. 

Wanneer u een van beide model training, moeten we normale gegevens, die niet nodig onderhoud of intrekken als de ingevoerde gegevens aan het model van de detectie PCA gebaseerde afwijking trainen gebruiken. In het score experiment gebruiken we het model van de detectie van ervaren afwijking om te bepalen of het voertuig is vereist voor onderhoud of intrekken of niet. 


### <a name="real-time-analysis"></a>Realtime analyse

De volgende Stream Analytics SQL-Query wordt gebruikt om het gemiddelde van alle belangrijke voertuig parameters zoals voertuigsnelheid, brandstofpeil engine temperatuur, kilometerstand, band druk, engine olive oil niveau en anderen. De gemiddelden worden gebruikt om te detecteren afwijkingen, actie-waarschuwingen, en bepalen van de algemene status voorwaarden van voertuigen dat wordt beheerd in specifieke regio en deze vervolgens overeenstemmen met doelgroepen. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig15-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

Figuur 15-Stream analytics-query voor realtime verwerking

Alle gemiddelden worden berekend via een TumblingWindow 3 seconden. We gebruiken TubmlingWindow in dit geval omdat we niet-overlappende en aaneengesloten tijdsintervallen vereist. 

Meer informatie over alle "Windowing"-mogelijkheden in Azure Stream analyses, klikt u op [Windowing (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).

**Realtime voorspellen**

Een toepassing maakt deel uit van de oplossing voor het model machine learning realtime mogelijk te maken. Deze toepassing 'RealTimeDashboardApp' genoemd is gemaakt en geconfigureerd als onderdeel van de oplossing-implementatie. De toepassing voert u het volgende:

1.  Luistert naar een gebeurtenis Hub exemplaar waar Stream Analytics de gebeurtenissen in een patroon continu publiceert. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-stream-analytics-query-for-publishing.png)*Afbeelding 16-Stream analytics-query voor het publiceren van de gegevens naar een uitvoer exemplaar van de gebeurtenis Hub* 

2.  Voor elke gebeurtenis die deze toepassing ontvangt: 

    - De gegevens met Machine Learning-aanvraag en respons scoren (records) eindpunt verwerkt. Het eindpunt van de records wordt automatisch gepubliceerd als onderdeel van de implementatie.
    - De uitvoer records wordt gepubliceerd naar een PowerBI gegevensset met de push-API's.

Dit patroon geldt ook voor scenario's waarin u wilt een lijn of Business (LoB)-toepassing integreren met de stroom realtime analyses, voor scenario's zoals waarschuwingen, meldingen en messaging.

Klik op [RealtimeDashboardApp downloaden](http://go.microsoft.com/fwlink/?LinkId=717078) als u wilt downloaden van de oplossing RealtimeDashboardApp Visual Studio voor aanpassingen. 

**De toepassing van de Dashboard Real-time uitvoeren**

1.  Klik op het knooppunt PowerBI in de diagramweergave te klikken en klik op de koppeling 'Downloaden Real-time dashboardtoepassing' in het deelvenster Eigenschappen. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17-vehicle-telematics-powerbi-dashboard-setup.png)*Afbeelding 17 – PowerBI-instructies voor het instellen van dashboard*
2.  Ophalen en lokaal opslaan ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-realtimedashboardapp-folder.png) *figuur 18 – RealtimeDashboardApp map*
3.  De toepassing RealtimeDashboardApp.exe uitvoeren
4.  Geldige Power BI-referenties opgeven, meld u aan en klik op accepteren ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png)![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

*Figuur 19 – RealtimeDashboardApp: Aanmelden bij PowerBI*

>[AZURE.NOTE] Als u leegmaken van de gegevensset PowerBI wilt, voert u de RealtimeDashboardApp met de parameter "flushdata" uit: 

    RealtimeDashboardApp.exe -flushdata

### <a name="batch-analysis"></a>Batchanalyse

Hier het doel is om weer te geven hoe Contoso Motors maakt gebruik van de mogelijkheden van de Azure berekeningscluster groot gegevens om te krijgen van de uitgebreide inzichten op patroon en gebruik gedrag voertuig systeemstatus sturende gebruik te maken. Hierdoor kunnen naar:

- Verbetering van de gebruikerservaring en goedkoper maken door te leveren inzichten op sturende voorkeur werkwijze en brandstof efficiënt groot gedrag
- Klanten en hun groot patters om te bepalen zakelijke beslissingen en het beste in klasse-producten en services bieden proactief te weten

In deze oplossing, we hebt samengesteld de doelstellingen van de volgende:

1.  **Agressieve groot gedrag**: geeft aan wat de trend van de modellen, locaties groot voorwaarden en tijd van de jaar-tot-inzicht op agressieve groot patronen krijgen. Contoso Motors kunt deze inzichten gebruik voor marketingcampagnes, sturende nieuwe persoonlijke functies en gebruik gebaseerde verzekeringen.
2.  **Brandstof efficiënt groot gedrag**: geeft aan wat de trend van de modellen, locaties groot voorwaarden en tijd van de jaar-tot-inzicht op brandstof efficiënt groot patronen krijgen. Contoso Motors kunt deze inzichten gebruik voor marketingcampagnes, sturende nieuwe functies en effectief en omgeving beschrijvende groot voorkeur werkwijze proactief rapporteren aan de stuurprogramma's voor kosten. 
3.  **Modellen intrekken**: modellen terughalen waarvoor de afwijking detectie machine learning-experiment dwarsas aangeeft

Laten we even naar op de details van elk van deze gegevens,


**Agressieve een patroon**

De gepartitioneerde voertuig signalen en diagnostische gegevens worden verwerkt in de pijplijn met de naam "AggresiveDrivingPatternPipeline" component gebruiken om te bepalen de modellen, locatie, voertuig, groot voorwaarden en andere parameters die agressieve een patroon vertoont.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-aggressive-driving-pattern.png) 
*Figuur 20 – agressief sturende patroon werkstroom*

De component script met de naam 'aggresivedriving.hql' gebruikt voor het analyseren van agressieve groot voorwaarde patroon bevindt zich op de map '\demo\src\connectedcar\scripts' van de gedownloade zip. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                vin                         string, 
                model                       string,
                timestamp                   string,
                city                        string,
                speed                       string,
                transmission_gear_position  string,
                brake_pedal_status          string,
                Year                        string,
                Month                       string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'

*Figuur 21 – agressief sturende patroon component query*

De combinatie van de overdracht tandwiel positie, de vorm status werking en de snelheid van voertuig wordt gebruikt om te bepalen roekeloze/agressieve groot gedrag op basis van het patroon hoge snelheid remmen. 

Zodra de pijplijn is uitgevoerd, kunt u de volgende partities gegenereerd in uw account opslag onder de container "connectedcar" zien.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aggressive-driving-pattern-output.png) 

*Figuur 22 – AggressiveDrivingPatternPipeline uitvoer*


**Brandstof efficiënt een patroon**

De gepartitioneerde voertuig signalen en diagnostische gegevens worden verwerkt in de pijplijn met de naam 'FuelEfficientDrivingPatternPipeline'. Component wordt gebruikt om te bepalen de modellen, locatie, voertuig, groot voorwaarden en andere eigenschappen die brandstof efficiënt een patroon vertonen.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-fuel-efficient-driving-pattern.png) 

*Figuur 23 – brandstof efficiënt een patroon werkstroom*

De component script met de naam 'fuelefficientdriving.hql' gebruikt voor het analyseren van agressieve groot voorwaarde patroon bevindt zich op de map '\demo\src\connectedcar\scripts' van de gedownloade zip. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                vin                         string, 
                model                       string,
                city                        string,
                speed                       string,
                transmission_gear_position  string,                
                brake_pedal_status          string,            
                accelerator_pedal_position  string,                             
                Year                        string,
                Month                       string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


*Figuur 24 – brandstof efficiënt een patroon component query*

Site gebruikmaakt van de combinatie van van voertuig overdracht tandwiel positie, werking vorm status, de snelheid en accelerator pedaal positie tot het brandstof efficiënt groot gedrag op basis van hardwareversnelling, remmen, opsporen en patronen sneller. 

Zodra de pijplijn is uitgevoerd, kunt u de volgende partities gegenereerd in uw account opslag onder de container "connectedcar" zien.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

*Afbeelding 25 – FuelEfficientDrivingPatternPipeline uitvoer*


**Voorspellingen intrekken**

De machine learning-experiment is ingericht en gepubliceerd als een webservice als onderdeel van de oplossing-implementatie. De batch scoren eindpunt wordt gebruikt in deze werkstroom geregistreerd als een gekoppeld factory-gegevensservice en geoperationaliseerd met gegevens factory batch scoren activiteit.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-machine-learning-endpoint.png) 

*Afbeelding 26 – Machine learning-eindpunt is geregistreerd als een gekoppelde service in gegevens fabriek*

De geregistreerde gekoppelde service wordt gebruikt in de DetectAnomalyPipeline voor het verkrijgen van de gegevens met het model van de detectie afwijking. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-aml-batch-scoring.png) 

*Afbeelding 27 – Azure Machine Learning Batch scoren activiteit in gegevens fabriek* 

Er zijn enkele stappen die worden uitgevoerd in deze pijplijn voor gegevensvoorbereiding van, zodat deze kan worden geoperationaliseerd met de batch scoren webservice. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-pipeline-predicting-recalls.png) 

*Afbeelding 28 – DetectAnomalyPipeline voor het voorspellen van voertuigen terughalen vereisen* 

Wanneer het scoren is voltooid, wordt de activiteit van een HDInsight wordt gebruikt om te verwerken en de gegevens die gecategoriseerd als afwijkingen door het model met een kans score van 0,60 of hoger zijn.

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                         string,
                model                       string,
                gendate                     string,
                outsidetemperature          string,
                enginetemperature           string,
                speed                       string,
                fuel                        string,
                engineoil                   string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position  string,
                parking_brake_status        string,
                headlamp_status             string,
                brake_pedal_status          string,
                transmission_gear_position  string,
                ignition_status             string,
                windshield_wiper_status     string,
                abs                         string,
                maintenanceLabel            string,
                maintenanceProbability      string,
                RecallLabel                 string,
                RecallProbability           string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                         string,
                model                       string,
                city                        string,
                outsidetemperature          string,
                enginetemperature           string,
                speed                       string,
                Year                        string,
                Month                       string              
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


Zodra de pijplijn is uitgevoerd, kunt u de volgende partities gegenereerd in uw account opslag onder de container "connectedcar" zien.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-detect-anamoly-pipeline-output.png) 

*Afbeelding 30 – afbeelding 30 – DetectAnomalyPipeline uitvoer*


## <a name="publish"></a>Publiceren

### <a name="real-time-analysis"></a>Realtime analyse

Een query in de stream analytics-taak de gebeurtenissen worden gepubliceerd naar een uitvoer exemplaar van de Hub van de gebeurtenis. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig31-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

*Afbeelding 31-Stream analytics taak worden gepubliceerd naar een uitvoer exemplaar van de gebeurtenis Hub*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig32-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

*Afbeelding 32 – Stream analytics query als u wilt publiceren naar de uitvoer exemplaar van de gebeurtenis Hub*

Deze stroom van gebeurtenissen wordt door de opgenomen in de oplossing RealTimeDashboardApp gebruikt. Deze toepassing maakt gebruik van de webservice Machine Learning verzoek-antwoord voor het realtime scoren en de resulterende gegevens worden gepubliceerd naar een gegevensset PowerBI voor verbruik. 

### <a name="batch-analysis"></a>Batchanalyse

De resultaten van de batch en realtime verwerking zijn gepubliceerd naar de tabellen Azure SQL-Database voor verbruik. De Azure SQL Server-Database en de tabellen worden automatisch gemaakt als onderdeel van het script instellen. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig33-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

*Afbeelding 33 – Batch processing resultaten kopiëren naar gegevens mart werkstroom*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig34-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

*Afbeelding 34 – Stream analytics taak worden gepubliceerd naar datamart*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig35-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

*Afbeelding 35 – datamart instellen in stream analytics taak*


## <a name="consume"></a>Gebruiken

Power BI krijgt deze oplossing een uitgebreide dashboard voor realtimegegevens en bekijk analyses visualisaties. 

Klik hier voor gedetailleerde instructies voor het instellen van de PowerBI-rapporten en het dashboard. Het uiteindelijke dashboard ziet er zo uit:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig36-vehicle-telematics-powerbi-dashboard.png)

*Afbeelding 36 - PowerBI-Dashboard*

## <a name="summary"></a>Overzicht

Dit document bevat een gedetailleerde Inzoomoptie van de oplossing voertuig Telemetrielogboek Analytics. Dit een patroon lambda architectuur voor gepresenteerd realtime en analyses met voorspellingen en acties batch. Dit patroon is van toepassing op een breed scala van gebruik aanvragen waarvoor warm pad (realtime) en koudwatersystemen pad (batch) analytics. 
